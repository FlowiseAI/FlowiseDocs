# 松弛 MCP

## 1.先决条件

在使用 Slack MCP 节点之前，您需要：

* **Slack 帐户**：在 [https://slack.com/](https://slack.com/) 处注册
* **Slack 工作区**
* **OAuth 客户端应用程序**：在 Slack 工作区中生成。这将为您提供 **客户端 ID** 和 **客户端密钥**。

***

## 2. 设置 Slack 凭据

### 2.1 在 Slack 中创建一个新应用

1. 转到[https://api.slack.com/apps/new](https://api.slack.com/apps/new)
2. 通过清单或从头开始创建一个新应用程序。

<figure><img src="../../../.gitbook/assets/image (174).png" alt=""><figcaption></figcaption></figure>

3. 创建应用程序后，获取客户端 ID 和客户端密钥。

<figure><img src="../../../.gitbook/assets/image (310).png" alt=""><figcaption></figcaption></figure>

### 2.2 在 Flowise 中添加凭据

1. 在 Flowise 中，从侧边栏导航至 **凭据**。
2. 单击“**添加凭据**”并搜索“**Slack 用户令牌 OAuth2**”。
3. 填写以下字段：

<table><thead><tr><th>领域</th><th>描述</th><th width="250">示例</th></tr></thead><tbody><tr><td><strong>客户ID</strong></td><td>Slack 中的 OAuth 客户端 ID</td><td><code>wBSGhxxxx</code></td></tr><tr><td><strong>客户秘密</strong></td><td>OAuth 客户端密钥（安全存储）</td><td><code>••••••••</code></td></tr><tr><td><strong>范围</strong></td><td><em>（可选）</em> 空格分隔的范围。</td><td><p><code>search:read.public search:read.private search:read.mpim search:read.im search:read.files search:read.users groups:history</code></p><p><code>mpim:history</code></p><p><code>im:history</code></p><p><code>channels:history</code></p><p><code>chat:write</code></p><p><code>canvases:read canvases:write</code></p><p><code>users:read</code></p><p><code>users:read.email</code></p></td></tr></tbody></table>

4. 复制 OAuth 重定向 URL，然后单击 **保存**。

<figure><img src="../../../.gitbook/assets/image (338).png" alt="" width="548"><figcaption></figcaption></figure>

**提示词：** 对于生产环境，请使用您需要的最窄范围。查看可用的[范围](https://docs.slack.dev/reference/scopes/)。

### 2.3 添加重定向 URL 到 Slack 应用程序

1. 从左侧导航栏中选择 OAuth & Permissions：

<figure><img src="../../../.gitbook/assets/image (340).png" alt=""><figcaption></figcaption></figure>

2. 向下滚动，您将看到重定向 URL 部分。添加从上一步复制的重定向 URL。然后单击保存 URL。

<figure><img src="../../../.gitbook/assets/image (341).png" alt="" width="563"><figcaption></figcaption></figure>

***

## 3. 添加松弛 MCP

1. 拖放一个 Agent 节点。
2. 添加新的 Slack MCP 工具。

<figure><img src="../../../.gitbook/assets/image (345).png" alt="" width="500"><figcaption></figcaption></figure>

3. 选择预配置的凭据，然后单击编辑按钮。单击“身份验证”。

<figure><img src="../../../.gitbook/assets/image (342).png" alt="" width="538"><figcaption></figcaption></figure>

3. 将出现一个 slack 弹出窗口，检查权限并单击允许。

<figure><img src="../../../.gitbook/assets/image (347).png" alt="" width="542"><figcaption></figcaption></figure>

4. 通过身份验证后，单击刷新按钮以加载可用的操作。

<figure><img src="../../../.gitbook/assets/image (348).png" alt=""><figcaption></figcaption></figure>

5. 选择操作。 _提示词：仅选择您的代理需要的操作。更少的工具可以帮助 LLM 做出更好的决策并减少代币使用。_

<figure><img src="../../../.gitbook/assets/image (339).png" alt="" width="452"><figcaption></figcaption></figure>

6.瞧！您可以开始与 Agent 聊天，看看它如何调用 Slack MCP 工具。

<figure><img src="../../../.gitbook/assets/image (349).png" alt=""><figcaption></figcaption></figure>
