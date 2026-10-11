---
description: 了解如何为 Flowise 实例设置应用程序级访问控制
---

# 应用

***

## 电子邮件和密码

从v3.0.1开始，引入了新的身份验证方法。 Flowise 使用 [**Passport.js**](https://www.passportjs.org/)** 基于身份验证系统**，并将 JWT 令牌存储在安全的 HTTP-only cookie 中。当用户登录时，系统使用 bcrypt 哈希比较来根据数据库验证其电子邮件/密码，然后生成两个 JWT 令牌：一个短期访问令牌（默认 60 分钟）和一个长期刷新令牌（默认 90 天）。这些令牌存储为安全 cookie。对于后续请求，系统从 cookie 中提取 JWT，使用 Passport 的 JWT 策略验证签名和声明，并检查用户会话是否仍然存在。系统还支持访问令牌过期时自动令牌刷新，根据配置使用 Redis 或数据库存储来维护会话。

对于一直使用[用户名和密码（已弃用）](app-level.md#username-and-password-deprecated)的现有用户，您需要设置一个新的管理员帐户。为了防止未经授权的所有权声明，您必须首先使用配置为 `FLOWISE_USERNAME` 和 `FLOWISE_PASSWORD` 的现有用户名和密码进行身份验证。

<figure><img src="../../.gitbook/assets/image (18) (1) (1).png" alt="" width="387"><figcaption></figcaption></figure>

可以更改以下环境变量：

### 应用程序 URL

* `APP_URL` - 您托管的 Flowise 应用程序 URL。默认为 `http://localhost:3000`

### JWT 环境变量配置

要配置 Flowise 的 JWT 身份验证参数，用户可以更改以下环境变量：

* `JWT_AUTH_TOKEN_SECRET` - 用于签署访问令牌的密钥
* `JWT_REFRESH_TOKEN_SECRET` - 刷新令牌的秘密（如果未设置，则默认为身份验证令牌秘密）
* `JWT_TOKEN_EXPIRY_IN_MINUTES` - 访问令牌生命周期（默认值：60 分钟）
* `JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES` - 刷新令牌生命周期（默认值：129,600 分钟或 90 天）
* `JWT_AUDIENCE` - 令牌验证受众声明（默认值：'AUDIENCE'）
* `JWT_ISSUER` - 令牌验证发行者声明（默认值：'ISSUER'）
* `EXPRESS_SESSION_SECRET` - 会话加密密钥（默认值：'flowise'）
* `EXPIRE_AUTH_TOKENS_ON_RESTART` - 设置为“true”以使服务器重新启动时的所有令牌无效（对开发有用）

### SMTP 电子邮件配置

配置这些变量以启用密码重置和通知的电子邮件功能：

* `SMTP_HOST` - SMTP 服务器的主机名（例如 `smtp.gmail.com`、`smtp.host.com`）
* `SMTP_PORT` - SMTP 连接的端口号（常用值：`587` 表示 TLS、`465` 表示 SSL、`25` 表示未加密）
* `SMTP_USER` - SMTP 身份验证的用户名（通常是您的电子邮件地址）
* `SMTP_PASSWORD` - 用于 SMTP 身份验证的密码或应用程序专用密码
* `SMTP_SECURE` - 对于 SSL/TLS 加密设置为 `true`，对于未加密连接设置为 `false`
* `ALLOW_UNAUTHORIZED_CERTS` - 设置为 `true` 以允许自签名证书（不建议用于生产）
* `SENDER_EMAIL` - 将显示在外发电子邮件上的“发件人”电子邮件地址

### 安全和令牌配置

这些变量控制身份验证安全性、令牌过期和密码散列：

* `PASSWORD_RESET_TOKEN_EXPIRY_IN_MINS` - 密码重置令牌的到期时间（默认值：15 分钟）
* `PASSWORD_SALT_HASH_ROUNDS` - 用于密码散列的 bcrypt salt 轮数（默认值：10，更高=更安全但更慢）
* `TOKEN_HASH_SECRET` - 用于哈希令牌和敏感数据的密钥（使用强随机字符串）

### 安全最佳实践

{% hint style="warning" %}
我们建议配置您自己的 JWT 和 Secret 令牌环境变量；否则，将使用默认值，这可能会增加攻击者伪造有效令牌并冒充用户的机会。
{% endhint %}

* 对 `TOKEN_HASH_SECRET` 使用强而独特的值并安全地存储它们
* 对于生产，请使用 `SMTP_SECURE=true` 和 `ALLOW_UNAUTHORIZED_CERTS=false`
* 根据您的安全要求设置适当的令牌到期时间
* 使用更高的 `PASSWORD_SALT_HASH_ROUNDS` 值 (12-15) 以提高生产安全性

## 用户名和密码（已弃用）

应用程序级授权通过用户名和密码保护您的 Flowise 实例。这可以防止您的应用程序在在线部署时被任何人访问。

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 如何设置用户名和密码

#### Npm

1.安装Flowise

```bash
npm install -g flowise
```

2. 使用用户名和密码启动 Flowise

```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

3. 打开[http://localhost:3000](http://localhost:3000)

#### 码头工人

1. 导航至 `docker` 文件夹

```
cd docker
```

2. 创建 `.env` 文件并指定 `PORT`、`FLOWISE_USERNAME` 和 `FLOWISE_PASSWORD`

```sh
PORT=3000
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```

3. 将 `FLOWISE_USERNAME` 和 `FLOWISE_PASSWORD` 传递到 `docker-compose.yml` 文件：

```
environment:
    - PORT=${PORT}
    - FLOWISE_USERNAME=${FLOWISE_USERNAME}
    - FLOWISE_PASSWORD=${FLOWISE_PASSWORD}
```

4.`docker compose up -d`
5. 打开[http://localhost:3000](http://localhost:3000)
6. 您可以通过 `docker compose stop` 将容器放下

#### Git 克隆

要启用应用程序级身份验证，请将 `FLOWISE_USERNAME` 和 `FLOWISE_PASSWORD` 添加到 `packages/server` 中的 `.env` 文件：

```
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```
