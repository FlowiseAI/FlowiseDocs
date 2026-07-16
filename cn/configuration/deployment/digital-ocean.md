---
description: 了解如何在 Digital Ocean 上部署 Flowise
---

# 数字海洋

***

## 创建水滴

在本节中，我们将创建一个 Droplet。如需了解更多信息，请参阅[官方指南](https://docs.digitalocean.com/products/droplets/quickstart/)。

1. 首先，从下拉列表中单击 **Droplets**

<figure><img src="../../.gitbook/assets/image (15) (2) (2).png" alt=""><figcaption></figcaption></figure>

2. 选择数据区域和基本 $6/月 Droplet 类型

<figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. 选择身份验证方法。在此示例中，我们将使用密码

<figure><img src="../../.gitbook/assets/image (5) (2).png" alt=""><figcaption></figcaption></figure>

4. 一段时间后，您应该能够看到 Droplet 创建成功

<figure><img src="../../.gitbook/assets/image (7) (2) (1).png" alt=""><figcaption></figcaption></figure>

## 如何连接到您的 Droplet

对于 Windows，请遵循此[指南](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/putty/)。

对于 Mac/Linux，请遵循此[指南](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/openssh/)。

## 安装 Docker

1.```
   卷曲-fsSL https://get.docker.com -o get-docker.sh
   ```
2. ```
   sudo sh get-docker.sh
   ```
3.安装docker-compose：

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

4.设置权限：

```
sudo chmod +x /usr/local/bin/docker-compose
```

## 设置

1. 克隆仓库

```
git clone https://github.com/FlowiseAI/Flowise.git
```

2. cd 进入 docker 文件夹

```bash
cd Flowise && cd docker
```

3. 创建 `.env` 文件。您可以使用您最喜欢的编辑器。我将使用 `nano`

```bash
nano .env
```

<figure><img src="../../.gitbook/assets/image (10) (2) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. 指定环境变量：

```sh
PORT=3000
DATABASE_PATH=/root/.flowise
SECRETKEY_PATH=/root/.flowise
LOG_PATH=/root/.flowise/logs
BLOB_STORAGE_PATH=/root/.flowise/storage
```

5. 然后按 `Ctrl + X` 退出，按 `Y` 保存文件
6.运行docker compose

```bash
docker compose up -d
```

7. 然后您可以查看应用程序：“您的公共 IPv4 DNS”：3000。示例：`176.63.19.226:3000`
8. 您可以通过以下方式关闭该应用程序：

```bash
docker compose stop
```

9. 您可以通过以下方式提取最新图像：

```bash
docker pull flowiseai/flowise
```

## 添加反向代理 & SSL

反向代理是将应用程序服务器公开到互联网的推荐方法。它将让我们单独使用 URL 而不是服务器 IP 和端口号连接到 Droplet。这提供了安全优势，将应用程序服务器与直接互联网访问隔离、集中防火墙保护的能力、针对常见威胁（例如拒绝服务攻击）的最小化攻击平面，以及对于我们的目的来说最重要的是，能够在单个位置终止 SSL/TLS 加密。

> Droplet 上缺少 SSL 将导致嵌入式小部件和 API 端点在现代浏览器中无法访问。这是因为浏览器已开始弃用 HTTP，转而使用 HTTPS，并阻止来自通过 HTTPS 加载的页面的 HTTP 请求。

### 步骤 1 — 安装 Nginx

1. Nginx 可以通过默认存储库使用 apt 安装。更新您的存储库索引，然后安装 Nginx：

```bash
sudo apt update
sudo apt install nginx
```

> 按 Y 确认安装。如果系统要求您重新启动服务，请按 ENTER 接受默认值。

2. 您需要允许通过防火墙访问Nginx。根据初始服务器先决条件设置服务器后，使用 ufw 添加以下规则：

```bash
sudo ufw allow 'Nginx HTTP'
```

3. 现在您可以验证 Nginx 是否正在运行：

```bash
systemctl status nginx
```

输出：

```bash
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2022-08-29 06:52:46 UTC; 39min ago
       Docs: man:nginx(8)
   Main PID: 9919 (nginx)
      Tasks: 2 (limit: 2327)
     Memory: 2.9M
        CPU: 50ms
     CGroup: /system.slice/nginx.service
             ├─9919 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             └─9920 "nginx: worker process
```

接下来，您将使用您的域和应用程序服务器代理添加自定义服务器块。

### 步骤 2 — 配置您的服务器块 + DNS 记录

建议为新添加的服务器块创建自定义配置文件，而不是直接编辑默认配置。

1. 使用 nano 或您喜欢的文本编辑器创建并打开新的 Nginx 配置文件：

```bash
sudo nano /etc/nginx/sites-available/your_domain
```

2. 将以下内容插入到新文件中，确保将 `your_domain` 替换为您自己的域名：

```
server {
    listen 80;
    listen [::]:80;
    server_name your_domain; #Example: demo.flowiseai.com
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_cache_bypass $http_upgrade;
    }
}
```

3.保存并退出，使用`nano`，您可以通过点击`CTRL+O`然后点击`CTRL+X`来完成此操作。
4. 接下来，通过创建一个链接来启用此配置文件，该链接指向 Nginx 在启动时读取的启用站点的目录，并再次确保将 `your_domain` 替换为您自己的域名：

```bash
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

5. 现在您可以测试配置文件是否存在语法错误：

```bash
sudo nginx -t
```

6. 如果没有报告任何问题，请重新启动 Nginx 以应用更改：

```bash
sudo systemctl restart nginx
```

7. 转到您的 DNS 提供商，然后添加新的 A 记录。名称将是您的域名，值将是您 Droplet 中的公共 IPv4 地址

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt="" width="367"><figcaption></figcaption></figure>

Nginx 现在已配置为应用程序服务器的反向代理。您现在应该可以打开应用程序：http://yourdomain.com.

### 步骤 3 — 为 HTTPS (SSL) 安装 Certbot

如果您想向 Droplet 添加安全的 `https` 连接（例如 https://yourdomain.com,），您需要执行以下操作：

1. 为了安装 Certbot 并在 NGINX 上启用 HTTPS，我们将依赖 Python。那么，首先我们来设置一个虚拟环境：

```bash
apt install python3.10-venv
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
```

2. 然后，运行以下命令安装 Certbot：

```bash
sudo /opt/certbot/bin/pip install certbot certbot-nginx
```

3. 现在，执行以下命令以确保可以运行 `certbot` 命令：

```bash
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

4. 最后，运行以下命令获取证书，并让Certbot自动修改NGINX配置，启用HTTPS：

```bash
sudo certbot --nginx
```

5. 按照证书生成向导操作后，我们将能够使用地址 https://yourdomain.com 通过 HTTPS 访问我们的 Droplet

### 设置自动续订

要使 Certbot 自动续订证书，只需运行以下命令来添加 cron 作业即可：

```bash
echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

## 恭喜！

您已在 Droplet 上成功设置 Flowise，并在您的域 [🥳](https://emojipedia.org/partying-face/) 上使用 SSL 证书

## 在 Digital Ocean 上更新 Flowise 的步骤

1. 导航到您安装 flowise 的目录

```bash
cd Flowise/docker
```

2.停止并删除docker镜像

注意：这不会删除您的流，因为数据库存储在单独的文件夹中

```bash
sudo docker compose stop
sudo docker compose rm
```

3.拉取最新的Flowise镜像

您可以在[此处](https://github.com/FlowiseAI/Flowise/releases)查看最新版本

```bash
docker pull flowiseai/flowise
```

4.启动docker

```bash
docker compose up -d
```
