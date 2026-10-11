---
description: 了解如何在 Azure 上部署 Flowise
---

# 天蓝色

***

## Flowise 作为 Azure 应用服务与 Postgres：使用 Terraform

### 先决条件

1. **Azure 帐户**：确保您拥有具有有效订阅的 Azure 帐户。如果您没有，请在 [Azure 门户](https://portal.azure.com/) 上注册。
2. **Terraform**：在您的计算机上安装 Terraform CLI。从[Terraform 的网站](https://www.terraform.io/downloads.html) 下载它。
3. **Azure CLI**：安装 Azure CLI。可以在 [Azure CLI 文档页面](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) 上找到相关说明。

### 设置您的环境

1. **登录 Azure**：打开终端或命令提示词符并使用以下命令登录 Azure CLI：

```bash
az login --tenant <Your Subscription ID> --use-device-code 
```

按照提示词完成登录过程。

2. **设置订阅**：登录后，使用以下命令设置 Azure 订阅：

```bash
az account set --subscription <Your Subscription ID>
```

3. **初始化 Terraform**：

在 Terraform 项目目录中创建一个 `terraform.tfvars` 文件（如果尚不存在），并添加以下内容：

```hcl
subscription_name = "subscrpiton_name"
subscription_id = "subscription id"
project_name = "webapp_name"
db_username = "PostgresUserName"
db_password = "strongPostgresPassword"
flowise_secretkey_overwrite = "longandStrongSecretKey"
webapp_ip_rules = [
  {
    name = "AllowedIP"
    ip_address = "X.X.X.X/32"
    headers = null
    virtual_network_subnet_id = null
    subnet_id = null
    service_tag = null
    priority = 300
    action = "Allow"
  }
]
postgres_ip_rules = {
  "ValbyOfficeIP" = "X.X.X.X"
  // Add more key-value pairs as needed
}
source_image = "flowiseai/flowise:latest"
tagged_image = "flow:v1"
```

将占位符替换为您的设置的实际值。

文件树结构如下：

```
flow
├── database.tf
├── main.tf
├── network.tf
├── output.tf
├── providers.tf
├── terraform.tfvars
├── terraform.tfvars.example
├── variables.tf
├── webapp.tf
├── .gitignore // ignore your .tfvars and .lock.hcf, .terraform

```

Terraform 配置中的每个 `.tf` 文件可能包含基础设施作为代码的不同方面：

<details>

<summary>`database.tf` 将定义 Postgres 数据库的配置。</summary>

```yaml

// database.tf

// Database instance
resource "azurerm_postgresql_flexible_server" "postgres" {
  name                         = "postgresql-${var.project_name}"
  location                     = azurerm_resource_group.rg.location
  resource_group_name          = azurerm_resource_group.rg.name
  sku_name                     = "GP_Standard_D2s_v3"
  storage_mb                   = 32768
  version                      = "11"
  delegated_subnet_id          = azurerm_subnet.dbsubnet.id
  private_dns_zone_id          = azurerm_private_dns_zone.postgres.id
  backup_retention_days        = 7
  geo_redundant_backup_enabled = false
  auto_grow_enabled            = false
  administrator_login          = var.db_username
  administrator_password       = var.db_password
  zone                         = "2"

  lifecycle {
    prevent_destroy = false
  }
}

// Firewall
resource "azurerm_postgresql_flexible_server_firewall_rule" "pg_firewall" {
  for_each         = var.postgres_ip_rules
  name             = each.key
  server_id        = azurerm_postgresql_flexible_server.postgres.id
  start_ip_address = each.value
  end_ip_address   = each.value
}

// Database
resource "azurerm_postgresql_flexible_server_database" "production" {
  name      = "production"
  server_id = azurerm_postgresql_flexible_server.postgres.id
  charset   = "UTF8"
  collation = "en_US.utf8"

  # prevent the possibility of accidental data loss
  lifecycle {
    prevent_destroy = false
  }
}

// Transport off
resource "azurerm_postgresql_flexible_server_configuration" "postgres_config" {
  name      = "require_secure_transport"
  server_id = azurerm_postgresql_flexible_server.postgres.id
  value     = "off"
}
```

</details>

<details>

<summary>`main.tf` 可能是主配置文件，其中可能包括 Azure 提供程序配置并定义 Azure 资源组。</summary>

```yaml
// main.tf
resource "random_string" "resource_code" {
  length  = 5
  special = false
  upper   = false
}

// resource group
resource "azurerm_resource_group" "rg" {
  location = var.resource_group_location
  name     = "rg-${var.project_name}"
}

// Storage Account
resource "azurerm_storage_account" "sa" {
  name                     = "${var.subscription_name}${random_string.resource_code.result}"
  resource_group_name      = azurerm_resource_group.rg.name
  location                 = azurerm_resource_group.rg.location
  account_tier             = "Standard"
  account_replication_type = "LRS"

  blob_properties {
    versioning_enabled = true
  }

}

// File share
resource "azurerm_storage_share" "flowise-share" {
  name                 = "flowise"
  storage_account_name = azurerm_storage_account.sa.name
  quota                = 50
}

```

</details>

<details>

<summary>`network.tf` 将包括网络资源，例如虚拟网络、子网和网络安全组。</summary>

```yaml
// network.tf

// Vnet
resource "azurerm_virtual_network" "vnet" {
  name                = "vn-${var.project_name}"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  address_space       = ["10.3.0.0/16"]
}

resource "azurerm_subnet" "dbsubnet" {
  name                                      = "db-subnet-${var.project_name}"
  resource_group_name                       = azurerm_resource_group.rg.name
  virtual_network_name                      = azurerm_virtual_network.vnet.name
  address_prefixes                          = ["10.3.1.0/24"]
  private_endpoint_network_policies_enabled = true
  delegation {
    name = "delegation"
    service_delegation {
      name = "Microsoft.DBforPostgreSQL/flexibleServers"
    }
  }
  lifecycle {
    ignore_changes = [
      service_endpoints,
      delegation
    ]
  }
}

resource "azurerm_subnet" "webappsubnet" {

  name                 = "web-app-subnet-${var.project_name}"
  resource_group_name  = azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.3.8.0/24"]

  delegation {
    name = "delegation"
    service_delegation {
      name = "Microsoft.Web/serverFarms"
    }
  }
  lifecycle {
    ignore_changes = [
      delegation
    ]
  }
}

resource "azurerm_private_dns_zone" "postgres" {
  name                = "private.postgres.database.azure.com"
  resource_group_name = azurerm_resource_group.rg.name
}

resource "azurerm_private_dns_zone_virtual_network_link" "postgres" {
  name                  = "private-postgres-vnet-link"
  resource_group_name   = azurerm_resource_group.rg.name
  private_dns_zone_name = azurerm_private_dns_zone.postgres.name
  virtual_network_id    = azurerm_virtual_network.vnet.id
}

```

</details>

<details>

<summary>`providers.tf` 将定义 Terraform 提供程序，例如 Azure。</summary>

```yaml
// providers.tf
terraform {
  required_version = ">=0.12"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=3.87.0"
    }
    random = {
      source  = "hashicorp/random"
      version = "~>3.0"
    }
  }
}

provider "azurerm" {
  subscription_id = var.subscription_id
  features {}
}
```

</details>

<details>

<summary>`variables.tf` 将声明在所有 `.tf` 文件中使用的变量。</summary>

```yaml
// variables.tf
variable "resource_group_location" {
  default     = "westeurope"
  description = "Location of the resource group."
}

variable "container_rg_name" {
  default     = "acrllm"
  description = "Name of container regrestry."
}

variable "subscription_id" {
  type        = string
  sensitive   = true
  description = "Service Subscription ID"
}

variable "subscription_name" {
  type        = string
  description = "Service Subscription Name"
}


variable "project_name" {
  type        = string
  description = "Project Name"
}

variable "db_username" {
  type        = string
  description = "DB User Name"
}

variable "db_password" {
  type        = string
  sensitive   = true
  description = "DB Password"
}

variable "flowise_secretkey_overwrite" {
  type        = string
  sensitive   = true
  description = "Flowise secret key"
}

variable "webapp_ip_rules" {
  type = list(object({
    name                      = string
    ip_address                = string
    headers                   = string
    virtual_network_subnet_id = string
    subnet_id                 = string
    service_tag               = string
    priority                  = number
    action                    = string
  }))
}

variable "postgres_ip_rules" {
  description = "A map of IP addresses and their corresponding names for firewall rules"
  type        = map(string)
  default     = {}
}

variable "flowise_image" {
  type        = string
  description = "Flowise image from Docker Hub"
}

variable "tagged_image" {
  type        = string
  description = "Tag for flowise image version"
}
```

</details>

<details>

<summary>`webapp.tf` Azure 应用服务，包括服务计划和 Linux Web 应用</summary>

```yaml
// webapp.tf
#Create the Linux App Service Plan
resource "azurerm_service_plan" "webappsp" {
  name                = "asp${var.project_name}"
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location
  os_type             = "Linux"
  sku_name            = "P3v3"
}

resource "azurerm_linux_web_app" "webapp" {
  name                = var.project_name
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location
  service_plan_id     = azurerm_service_plan.webappsp.id

  app_settings = {
    DOCKER_ENABLE_CI                    = true
    WEBSITES_CONTAINER_START_TIME_LIMIT = 1800
    WEBSITES_ENABLE_APP_SERVICE_STORAGE = false
    DATABASE_TYPE                       = "postgres"
    DATABASE_HOST                       = azurerm_postgresql_flexible_server.postgres.fqdn
    DATABASE_NAME                       = azurerm_postgresql_flexible_server_database.production.name
    DATABASE_USER                       = azurerm_postgresql_flexible_server.postgres.administrator_login
    DATABASE_PASSWORD                   = azurerm_postgresql_flexible_server.postgres.administrator_password
    DATABASE_PORT                       = 5432
    FLOWISE_SECRETKEY_OVERWRITE         = var.flowise_secretkey_overwrite
    PORT                                = 3000
    SECRETKEY_PATH                      = "/root"
    DOCKER_IMAGE_TAG                    = var.tagged_image
  }

  storage_account {
    name         = "${var.project_name}_mount"
    access_key   = azurerm_storage_account.sa.primary_access_key
    account_name = azurerm_storage_account.sa.name
    share_name   = azurerm_storage_share.flowise-share.name
    type         = "AzureFiles"
    mount_path   = "/root"
  }


  https_only = true

  site_config {
    always_on              = true
    vnet_route_all_enabled = true
    dynamic "ip_restriction" {
      for_each = var.webapp_ip_rules
      content {
        name       = ip_restriction.value.name
        ip_address = ip_restriction.value.ip_address
      }
    }
    application_stack {
      docker_image_name        = var.flowise_image
      docker_registry_url      = "https://${azurerm_container_registry.acr.login_server}"
      docker_registry_username = azurerm_container_registry.acr.admin_username
      docker_registry_password = azurerm_container_registry.acr.admin_password
    }
  }

  logs {
    http_logs {
      file_system {
        retention_in_days = 7
        retention_in_mb   = 35
      }

    }
  }

  identity {
    type = "SystemAssigned"
  }

  lifecycle {
    create_before_destroy = false

    ignore_changes = [
      virtual_network_subnet_id
    ]
  }

}

resource "azurerm_app_service_virtual_network_swift_connection" "webappvnetintegrationconnection" {
  app_service_id = azurerm_linux_web_app.webapp.id
  subnet_id      = azurerm_subnet.webappsubnet.id

  depends_on = [azurerm_linux_web_app.webapp, azurerm_subnet.webappsubnet]
}

```

</details>

注意： `.terraform` 目录是 Terraform 在初始化项目 (`terraform init`) 时创建的，它包含 Terraform 运行所需的插件和二进制文件。 `.terraform.lock.hcl` 文件用于记录确切的提供程序版本，这些版本用于确保不同计算机上的安装一致。

导航到您的 Terraform 项目目录并运行：

```bash
terraform init
```

这将初始化 Terraform 并下载所需的提供程序。

### 配置 Terraform 变量

### 使用 Terraform 进行部署

1. **规划部署**：运行 Terraform plan 命令以查看将创建哪些资源：

    ```bash
    terraform plan
    ```
2. **应用部署**：如果您对计划感到满意，请应用更改：

    ```bash
    terraform apply
    ```

    出现提示词时确认操作，Terraform 将开始创建资源。
3. **验证部署**：Terraform 完成后，它将输出任何定义的输出，例如 IP 地址或域名。验证资源是否已正确部署在 Azure 门户中。

***

## Azure Continer 实例：使用 Azure 门户 UI 或 Azure CLI

### 先决条件

1. _（可选）_ [如果您想遵循基于 cli 的命令，请安装 Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)

## 创建一个没有持久存储的容器实例

如果没有持久存储，您的数据将保存在内存中。这意味着容器重新启动后，您存储的所有数据都将消失。

### 在门户中

1、在Marketplace中搜索容器实例，点击创建：

<figure><img src="../../.gitbook/assets/azure/deployment/1.png" alt=""><figcaption><p>Azure 市场中的容器实例条目</p></figcaption></figure>

2. 选择或创建资源组、容器名称、区域、镜像源`Other registry`、镜像类型、镜像`flowiseai/flowise`、操作系统类型和大小。然后点击“下一步：网络”配置 Flowise 端口：

<figure><img src="../../.gitbook/assets/azure/deployment/2.png" alt=""><figcaption><p>容器实例创建向导中的第一页</p></figcaption></figure>

3. 在默认端口 `80 (TCP)` 旁边添加新端口 `3000 (TCP)`。然后选择“下一步：高级”：

<figure><img src="../../.gitbook/assets/azure/deployment/3.png" alt=""><figcaption><p>容器实例创建向导中的第二页。它要求网络类型和端口。</p></figcaption></figure>

4. 将重新启动策略设置为 `On failure`。添加命令覆盖 `["/bin/sh", "-c", "flowise start"]`。最后点击“审核+创建”：

<figure><img src="../../.gitbook/assets/azure/deployment/4.png" alt=""><figcaption><p>容器实例创建向导中的第三页。它要求重新启动策略、环境变量和在容器启动时运行的命令。</p></figcaption></figure>

5. 检查最终设置并单击“创建”：

<figure><img src="../../.gitbook/assets/azure/deployment/5.png" alt=""><figcaption><p>容器实例的最终审核和创建页面。</p></figcaption></figure>

6.创建完成后，点击“转到资源”

<figure><img src="../../.gitbook/assets/azure/deployment/6.png" alt=""><figcaption><p>Azure 中的资源创建结果页面。</p></figcaption></figure>

7. 通过复制 IP 地址并添加 :3000 作为端口来访问您的 Flowise 实例：

<figure><img src="../../.gitbook/assets/azure/deployment/7.png" alt=""><figcaption><p>容器实例概览页面</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/azure/deployment/8.png" alt=""><figcaption><p>Flowise 应用程序部署为容器实例</p></figcaption></figure>

### 使用 Azure CLI 创建

1. 创建一个资源组（如果您还没有）

```bash
az group create --name flowise-rg --location "West US"
```

2. 创建容器实例

```bash
az container create -g flowise-rg \
	--name flowise \
	--image flowiseai/flowise \
	--command-line "/bin/sh -c 'flowise start'" \
	--ip-address public \
	--ports 80 3000 \
	--restart-policy OnFailure
```

3. 访问上述命令输出中打印的 IP 地址（包括端口：3000）。

## 创建具有持久存储的容器实例

只能使用 CLI 创建具有持久存储的容器实例：

1. 创建一个资源组（如果您还没有）

```bash
az group create --name flowise-rg --location "West US"
```

2. 在上述资源组内创建存储帐户资源（或使用现有资源）。您可以在[此处](https://learn.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)查看操作方法。
3. 在 Azure 存储中创建新的文件共享。您可以在[此处](https://learn.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)查看操作方法。
4. 创建容器实例

```bash
az container create -g flowise-rg \
	--name flowise \
	--image flowiseai/flowise \
	--command-line "/bin/sh -c 'flowise start'" \
	--environment-variables DATABASE_PATH=/opt/flowise/.flowise SECRETKEY_PATH=/opt/flowise/.flowise LOG_PATH=/opt/flowise/.flowise/logs BLOB_STORAGE_PATH=/opt/flowise/.flowise/storage \
	--ip-address public \
	--ports 80 3000 \
	--restart-policy OnFailure \
	--azure-file-volume-share-name here goes the name of your File share \
	--azure-file-volume-account-name here goes the name of your Storage Account \
	--azure-file-volume-account-key here goes the access key to your Storage Account \
	--azure-file-volume-mount-path /opt/flowise/.flowise
```

5. 访问上述命令输出中打印的 IP 地址（包括端口：3000）。
6. 从现在开始，您的数据将存储在 SQLite 数据库中，您可以在文件共享中找到该数据库。

观看有关部署到 Azure 容器实例的视频教程：

{% embed url="https://www.youtube.com/watch?v=yDebxDfn2yk" %}
