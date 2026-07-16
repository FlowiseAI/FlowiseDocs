# 邮箱

## 在 Flowise 中创建凭据

1. 添加新的 Gmail OAuth2 凭据
2. 输入凭据的名称。
3. 复制 OAuth 重定向 URL。
4. 请注意，需要填写以下字段：
   * 客户ID
   * 客户秘密

<figure><img src="../../../.gitbook/assets/image (255).png" alt="" width="437"><figcaption></figcaption></figure>

## 创建/使用 Google 项目

1. 登录您的 [**Google Cloud**](https://console.cloud.google.com/) 帐户。
2. 导航至 [**Google Cloud Console > API 和服务**](https://console.cloud.google.com/apis/credentials)，然后从左上角的下拉列表中选择您要使用的项目（或创建一个新项目并选择它）。
3. 如果您之前未配置过，请设置 **OAuth 同意屏幕**。

<figure><img src="../../../.gitbook/assets/image (256).png" alt="" width="563"><figcaption></figcaption></figure>

4. 转至 **凭据**，然后单击 **+ CREATE CREDENTIALS > OAuth 客户端 ID**。

<figure><img src="../../../.gitbook/assets/image (257).png" alt="" width="563"><figcaption></figcaption></figure>

5. 在 **应用程序类型** 下拉列表中，选择 **Web 应用程序**。
6. 在 **授权重定向 URI** 下，单击 **+ ADD URI** 并粘贴之前复制的 OAuth 重定向 URL。
7. 单击“**创建**”。

<figure><img src="../../../.gitbook/assets/image (258).png" alt="" width="407"><figcaption></figcaption></figure>

8. 复制客户端 ID 和客户端密钥：

<figure><img src="../../../.gitbook/assets/image (259).png" alt="" width="489"><figcaption></figcaption></figure>

9. 在**启用的 API 和服务**中，单击 **+ ENABLE APIS AND SERVICES**。
10. 搜索并启用 **Gmail API**。

<figure><img src="../../../.gitbook/assets/image (260).png" alt="" width="538"><figcaption></figcaption></figure>

11. 返回**凭据**，单击 **OAuth 2.0 客户端 ID** 下新创建的凭据，在详细信息页面上，您将找到 **客户端 ID** 和 **客户端密钥**。

## 在 Flowise 中完成设置

1. 填写之前复制的所有值。然后点击“**验证**”：

<figure><img src="../../../.gitbook/assets/image (262).png" alt="" width="433"><figcaption></figcaption></figure>

2. 会弹出Google登录窗口：

<figure><img src="../../../.gitbook/assets/image (261).png" alt="" width="448"><figcaption></figcaption></figure>

3.授予权限：

<figure><img src="../../../.gitbook/assets/image (263).png" alt="" width="373"><figcaption></figcaption></figure>

4. 弹出窗口将自动关闭，凭据将被保存并可供使用。

## 用作代理工具

可以选择多种操作，让代理智能地选择合适的操作。\
参数可以留空，以允许代理自行确定值。但是，如果用户提供值，这些值将覆盖代理的选择。

<figure><img src="../../../.gitbook/assets/image (264).png" alt=""><figcaption></figcaption></figure>

## 用作工具节点

它还可以在确定的工作流程场景中用作工具节点。例如，在继续下一步之前检索草稿消息列表。\
在此模式下，**工具输入参数必须显式定义并填充值**。\
与 [**用作代理工具**](gmail.md#use-as-agent-tool) 选项不同，没有代理可以自动确定输入。用户必须手动填充字段，方法是输入固定值或使用双大括号 `{{ }}` 中包含的变量。

<figure><img src="../../../.gitbook/assets/image (265).png" alt=""><figcaption></figcaption></figure>
