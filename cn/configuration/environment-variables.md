---
description: 了解如何为 Flowise 配置环境变量
---

# 环境变量

Flowise 支持不同的环境变量来配置您的实例。您可以在 `packages/server` 文件夹内的 `.env` 文件中指定以下变量。请参阅 [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/.env.example) 文件。

<table><thead><tr><th width="233">变量</th><th width="219">描述</th><th width="104">类型</th><th>默认</th></tr></thead><tbody><tr><td>PORT</td><td>Flowise 运行于 HTTP 端口</td><td>数量</td><td>3000</td></tr><tr><td>FLOWISE_FILE_SIZE_LIMIT</td><td>上传时最大文件大小</td><td>字符串</td><td><code>50mb</code></td></tr><tr><td>NUMBER_OF_PROXIES</td><td>速率限制代理</td><td>数量</td><td></td></tr><tr><td>CORS_ORIGINS</td><td>所有跨域 HTTP 调用的允许来源</td><td>字符串</td><td></td></tr><tr><td>IFRAME_ORIGINS</td><td>iframe src 嵌入允许的来源</td><td>字符串</td><td></td></tr><tr><td>SHOW_COMMUNITY_NODES</td><td>显示社区创建的节点</td><td>布尔值： <code>true</code> 或 <code>false</code></td><td></td></tr><tr><td>DISABLED_NODES</td><td>要禁用的节点名称的逗号分隔列表</td><td>字符串</td><td></td></tr></tbody></table>

## 对于数据库

|变量|描述 |类型 |默认|
| ------------------ | ---------------------------------------------------------------- | ------------------------------------------ | ------------------------ |
| DATABASE\_类型 |存储流程数据的数据库类型 |枚举字符串：`sqlite`、`mysql`、`postgres` | `sqlite` |
| DATABASE\_PATH |数据库保存位置（当DATABASE\_TYPE为sqlite时） |字符串| `your-home-dir/.flowise` |
| DATABASE\_HOST | DATABASE\_HOST |主机 URL 或 IP 地址（当 DATABASE\_TYPE 不是 sqlite 时）|字符串|                          |
| DATABASE\_端口 |数据库端口（当 DATABASE\_TYPE 不是 sqlite 时）|字符串|                          |
| DATABASE\_USER |数据库用户名（当 DATABASE\_TYPE 不是 sqlite 时）|字符串|                          |
| DATABASE\_密码 |数据库密码（当 DATABASE\_TYPE 不是 sqlite 时）|字符串|                          |
| DATABASE\_NAME |数据库名称（当 DATABASE\_TYPE 不是 sqlite 时）|字符串|                          |
| DATABASE\_SSL |需要数据库 SSL （当 DATABASE\_TYPE 不是 sqlite 时）|布尔值：`true` 或 `false` | `false` |

## 用于存储

Flowise 默认将以下文件存储在本地路径文件夹下。

* 文件上传到[文档加载器](../integrations/langchain/document-loaders/)/文档存储
* 从聊天上传图像/音频
* 来自助手的图像/文件
* 来自 [Vector 写入更新 API](/broken/pages/F2AfRpI7qYixNiBWpmIe#vector-upsert-api) 的文件

用户可以指定 `STORAGE_TYPE` 以使用 AWS S3、Google Cloud Storage 或本地路径

|变量|描述 |类型 |默认|
| -------------------------------------- | -------------------------------------------------------------------------------- | --------------------------------- | -------------------------------- |
| STORAGE\_类型 |上传文件的存储类型。默认为 `local` |枚举字符串：`s3`、`gcs`、`local` | `local` |
| BLOB\_STORAGE\_PATH | `STORAGE_TYPE` 为 `local` 时存储上传文件的本地文件夹路径 |字符串| `your-home-dir/.flowise/storage` |
| S3\_STORAGE\_BUCKET\_NAME |当 `STORAGE_TYPE` 为 `s3` 时保存上传文件的存储桶名称 |字符串|                                  |
| S3\_STORAGE\_ACCESS\_KEY\_ID | AWS 访问密钥 |字符串|                                  |
| S3\_存储\_秘密\_访问\_密钥 | AWS 密钥 |字符串|                                  |
| S3\_存储区域 | S3 存储桶的区域 |字符串|                                  |
| S3\_ENDPOINT\_URL |自定义 S3 端点（可选）|字符串|                                  |
| S3\_FORCE\_PATH\_STYLE |强制 S3 路径样式（可选）|布尔 |假 |
| GOOGLE\_CLOUD\_STORAGE\_CREDENTIAL |谷歌云服务帐户密钥 |字符串|                                  |
| GOOGLE\_CLOUD\_STORAGE\_PROJ\_ID |谷歌云项目ID |字符串|                                  |
| GOOGLE\_CLOUD\_STORAGE\_BUCKET\_NAME | Google 云存储桶名称 |字符串|                                  |
| GOOGLE\_云\_统一\_桶\_访问|访问类型 |布尔 |真实 |

## 用于调试和日志

|变量|描述 |类型 |                                |
| ---------- | ----------------------------------- | ------------------------------------------------ | ------------------------------ |
| DEBUG |从组件打印日志 |布尔 |                                |
| LOG\_PATH |日志文件存储位置 |字符串| `Flowise/packages/server/logs` |
| LOG\_LEVEL |不同级别的日志 |枚举字符串：`error`、`info`、`verbose`、`debug` | `info` |

`DEBUG`：如果设置为 true，会将日志打印到终端/控制台：

<figure><img src="../.gitbook/assets/image (3) (3) (1).png" alt=""><figcaption></figcaption></figure>

`LOG_LEVEL`：要保存的记录器的不同日志级别。可以是 `error`、`info`、`verbose` 或 `debug.` 默认情况下，它设置为 `info,`，仅将 `logger.info` 保存到日志文件中。如果您想获得完整的详细信息，请设置为 `debug`。

<figure><img src="../.gitbook/assets/image (2) (4).png" alt=""><figcaption><p><strong>server-requests.log.jsonl - 记录发送到 Flowise 的每个请求</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><strong>server.log - 记录 Flowise 上的一般操作</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (4).png" alt=""><figcaption><p><strong>server-error.log - 使用堆栈跟踪记录错误</strong></p></figcaption></figure>

### 日志流 S3

当 `STORAGE_TYPE` 环境变量设置为 `s3` 时，日志将自动流式传输并存储到 S3。新的日志文件将每小时创建一次，以便更轻松地进行调试。

### 日志流 GCS

当 `STORAGE_TYPE` 环境变量设置为 `gcs` 时，日志将自动传输到 Google [Cloud Logging](https://cloud.google.com/logging?hl=en)。

## 凭据

Flowise 使用加密密钥将您的第三方 API 密钥存储为加密凭据。

默认情况下，启动应用程序时将生成随机加密密钥并存储在文件路径下。然后每次都会检索该加密密钥以解密聊天流中使用的凭据。例如，您的 OpenAI API 密钥、Pinecone API 密钥等。

您可以配置为使用 AWS Secret Manager 来存储加密密钥。

|变量|描述 |类型 |默认 |
| ----------------------------- | ----------------------------------------------------- | --------------------------- | ------------------------- |
| SECRETKEY\_存储\_类型 |如何存储加密密钥 |枚举字符串：`local`、`aws` | `local` |
| SECRETKEY\_PATH |保存加密密钥的本地文件路径 |字符串| `Flowise/packages/server` |
| FLOWISE\_SECRETKEY\_OVERWRITE |使用加密密钥代替现有密钥 |字符串|                           |
| SECRETKEY\_AWS\_ACCESS\_KEY |                                                       |字符串|                           |
| SECRETKEY\_AWS\_SECRET\_KEY |                                                       |字符串|                           |
| SECRETKEY\_AWS\_REGION |                                                       |字符串|                           |

由于某些原因，有时可能会重新生成加密密钥或更改存储路径，这会导致错误，例如 - <mark style="color:red;">凭据无法解密。</mark>

为了避免这种情况，您可以将自己的加密密钥设置为 `FLOWISE_SECRETKEY_OVERWRITE`，这样每次都会使用相同的加密密钥。格式没有限制，您可以将其设置为任何您想要的文本，或者与您的 `FLOWISE_PASSWORD` 相同。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
从 UI 返回的凭据 API 密钥与您设置的原始 Api 密钥长度不同。这是一个伪造的前缀字符串，可以防止网络欺骗，这就是我们不将 Api 密钥返回给 UI 的原因。但是，在您与聊天流交互期间，将检索并使用正确的 Api 密钥。
{% endhint %}

## 对于模型

在某些情况下，您可能希望在现有聊天模型和 LLM 节点上使用自定义模型，或限制仅访问某些模型。

默认情况下，Flowise 从[此处](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/models.json) 提取模型列表。但是，用户可以创建自己的 `models.json` 文件并指定文件路径：

<table><thead><tr><th width="164">变量</th><th width="196">描述</th><th width="78">类型</th><th>默认</th></tr></thead><tbody><tr><td>MODEL_LIST_CONFIG_JSON</td><td>从您的加载模型列表的链接 <code>models.json</code> 配置文件</td><td>字符串</td><td><a href="https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json">https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json</a></td></tr></tbody></table>

## 对于内置和外部依赖项

Flowise 中的某些节点/功能允许用户运行 Javascript 代码。出于安全原因，默认情况下它只允许某些依赖项。通过设置以下环境变量可以解除内置和外部模块的限制：

<table><thead><tr><th>变量</th><th width="300.4444580078125">描述</th><th></th></tr></thead><tbody><tr><td>TOOL_FUNCTION_BUILTIN_DEP</td><td>NodeJS 内置模块的使用</td><td>字符串</td></tr><tr><td>TOOL_FUNCTION_EXTERNAL_DEP</td><td>使用的外部模块 </td><td>字符串</td></tr><tr><td>ALLOW_BUILTIN_DEP</td><td>允许使用项目依赖项，例如 <code>cheerio</code>, <code>typeorm</code></td><td>布尔值</td></tr></tbody></table>

{% code title=".env" %}
```bash
# Allows usage of all builtin modules
TOOL_FUNCTION_BUILTIN_DEP=*

# Allows usage of only fs
TOOL_FUNCTION_BUILTIN_DEP=fs

# Allows usage of only crypto and fs
TOOL_FUNCTION_BUILTIN_DEP=crypto,fs

# Allow usage of external npm modules.
TOOL_FUNCTION_EXTERNAL_DEP=cheerio,typeorm

ALLOW_BUILTIN_DEP=true
```
{% endcode %}

### 使用内置依赖项

{% hint style="warning" %}
某些内置依赖项（例如 Puppeteer）可能会引入潜在的安全漏洞。建议在使用前仔细分析和评估这些风险。
{% endhint %}

### NodeVM 执行错误：VMError：找不到模块

如果您使用默认情况下不允许的库，您可以：

1. 允许所有项目的 [库/依赖项](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L52)：`ALLOW_BUILTIN_DEP=true`
2.（推荐）特别允许某些库/依赖项：`TOOL_FUNCTION_EXTERNAL_DEP=cheerio,typeorm`

## 安全配置

<table><thead><tr><th width="246.4444580078125">变量</th><th width="180.4444580078125">描述</th><th width="192.666748046875">选项</th><th>默认</th></tr></thead><tbody><tr><td><code>HTTP_DENY_LIST</code></td><td>阻止对 MCP 服务器中指定 URL 或域的 HTTP 请求</td><td>以逗号分隔的 URL/域</td><td><em>（空）</em></td></tr><tr><td><code>CUSTOM_MCP_SECURITY_CHECK</code></td><td>为自定义 MCP 配置启用全面的安全验证</td><td><code>true</code> | <code>false</code></td><td><code>true</code></td></tr><tr><td><code>CUSTOM_MCP_PROTOCOL</code></td><td>设置自定义 MCP 通信的默认协议</td><td><code>stdio</code> |<code>sse</code></td><td><code>stdio</code></td></tr></tbody></table>

#### `CUSTOM_MCP_SECURITY_CHECK=true`

默认情况下，此功能已启用。启用后，应用以下安全验证：

* **命令白名单**：仅允许安全命令（`node`、`npx`、`python`、`python3`、`docker`）
* **参数验证**：阻止危险的文件路径、目录遍历和可执行文件
* **注入预防**：防止 shell 元字符和命令链
* **环境保护**：阻止修改关键环境变量（PATH、LD\_LIBRARY\_PATH）

#### `CUSTOM_MCP_PROTOCOL`

* **`stdio`**：直接进程通信（默认，需要命令执行）
* **`sse`**：服务器通过 HTTP 发送的事件（推荐用于生产，更安全）

### 推荐的生产设置

```bash
# Enable security validation (default)
CUSTOM_MCP_SECURITY_CHECK=true

# Use SSE protocol for better security
CUSTOM_MCP_PROTOCOL=sse

# Block dangerous domains (example)
HTTP_DENY_LIST=localhost,127.0.0.1,internal.company.com

# Blocks a hardcoded list of dangerous domains by default, but can be set to false to disable
HTTP_SECURITY_CHECK=true

# Enables checks on provided file and folder paths to prevent path traversal attacks
PATH_TRAVERSAL_SAFETY=true
```

{% hint style="warning" %}
**警告**：禁用 `CUSTOM_MCP_SECURITY_CHECK` 会导致任意命令执行，并在生产环境中带来重大安全风险。

`HTTP_SECURITY_CHECK` 启用内置安全功能，阻止硬编码的危险域列表。默认情况下为 `true`，可以通过将其设置为 `false` 来禁用。

`HTTP_DENY_LIST` 允许您指定要阻止的附加自定义域列表。默认情况下，此列表为空。

`PATH_TRAVERSAL_SAFETY` 启用内置安全功能来防止对文件和文件夹路径的路径遍历攻击。默认情况下为 `true`，可以通过将其设置为 `false` 来禁用。
{% endhint %}

## 如何设置环境变量的示例

### NPM

您可以在使用 npx 运行 Flowise 时设置所有这些变量。例如：

```
npx flowise start --PORT=3000 --DEBUG=true
```

### 码头工人

```
docker run -d -p 5678:5678 flowise \
 -e DATABASE_TYPE=postgresdb \
 -e DATABASE_PORT=<POSTGRES_PORT> \
 -e DATABASE_HOST=<POSTGRES_HOST> \
 -e DATABASE_NAME=<POSTGRES_DATABASE_NAME> \
 -e DATABASE_USER=<POSTGRES_USER> \
 -e DATABASE_PASSWORD=<POSTGRES_PASSWORD> \
```

### Docker 组合

您可以在 `docker` 文件夹内的 `.env` 文件中设置所有这些变量。请参阅 [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example) 文件。
