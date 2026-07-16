# 泽普记忆

[Zep](https://github.com/getzep/zep) 是 LLM 应用程序的长期内存存储。它存储、总结、嵌入、索引和丰富 LLM 应用程序/聊天机器人历史记录，并通过简单、低延迟的 API 公开它们。

## 部署 Zep 渲染指南

您可以轻松地将 Zep 部署到云服务，例如 [Render](https://render.com/)、[Flyio](https://fly.io/)。如果您希望在本地进行测试，还可以按照[快速指南](https://github.com/getzep/zep#quick-start)启动 Docker 容器。

在此示例中，我们将部署到渲染。

1. 前往 [Zep Repo](https://github.com/getzep/zep#quick-start) 并单击 **部署到渲染**
2. 这将带您进入渲染的蓝图页面，然后只需单击 **创建新资源**

<figure><img src="../../../.gitbook/assets/image (21) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. 部署完成后，您应该会在仪表板上看到创建的 3 个应用程序

<figure><img src="../../../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

4. 只需单击第一个名为 **zep** 并复制部署的 URL

<figure><img src="../../../.gitbook/assets/image (38) (1).png" alt=""><figcaption></figcaption></figure>

## 将 Zep 部署到 Digital Ocean 的指南（通过 Docker）

1. 克隆存储库

```bash
git clone https://github.com/getzep/zep.git
cd zep
nano .env

```

2. 添加您的 OpenAI API 密钥.ENV

```bash
ZEP_OPENAI_API_KEY=

```

```bash
docker compose up -d --build
```

3.允许防火墙访问8000端口

```bash
sudo ufw allow from any to any port 8000 proto tcp
ufw status numbered
```

如果使用数字海洋与仪表板分开的防火墙，请确保端口 8000 也添加在那里

## 在 Flowise UI 中使用

1. 返回 Flowise 应用程序，只需创建一个新画布或使用市场中的模板之一。在此示例中，我们将使用 **简单对话链**

<figure><img src="../../../.gitbook/assets/Untitled (3) (1).png" alt=""><figcaption></figcaption></figure>

2. 将 **缓冲内存** 替换为 **Zep 内存**。然后将 **Base URL** 替换为您在上面复制的 Zep URL

<figure><img src="../../../.gitbook/assets/Untitled (5).png" alt=""><figcaption></figcaption></figure>

3. 保存聊天流并进行测试以查看对话是否被记住。

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

4. 现在尝试清除聊天记录，您应该会看到它现在无法记住以前的对话。

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Zep 身份验证

Zep 允许您使用 JWT 身份验证来保护您的实例。我们将使用 `zepcli` 命令行实用程序[此处](https://github.com/getzep/zepcli/releases)。

#### 1. 生成秘密和 JWT 令牌<a href="#id-1-generate-a-secret-and-the-jwt-token" id="id-1-generate-a-secret-and-the-jwt-token"></a>

下载 ZepCLI 后：

在 Linux 或 MacOS 上

```
./zepcli -i
```

在 Windows 上

```
zepcli.exe -i
```

您将首先获得您的 SECRET 令牌：

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

然后您将获得 JWT 代币：

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### 2.配置Auth环境变量<a href="#id-2-configure-auth-environment-variables" id="id-2-configure-auth-environment-variables"></a>

在 Zep 服务器环境中设置以下环境变量：

```
ZEP_AUTH_REQUIRED=true
ZEP_AUTH_SECRET=<the secret you generated above>
```

#### 3. 在 Flowise 上配置凭据<a href="#id-2-configure-auth-environment-variables" id="id-2-configure-auth-environment-variables"></a>

为 Zep 添加新凭据，并将 JWT 令牌放入 API 密钥 字段中：

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 4. 在 Zep 节点上使用创建的凭据<a href="#id-2-configure-auth-environment-variables" id="id-2-configure-auth-environment-variables"></a>

在 Zep 节点连接凭据中，选择您刚刚创建的凭据。就是这样！

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
