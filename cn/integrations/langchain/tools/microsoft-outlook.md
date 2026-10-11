# 微软 Outlook

## 先决条件

分配给 Azure Active Directory 用户的有效 Microsoft 365 许可证。请参阅：[https://learn.microsoft.com/en-us/answers/questions/761931/microsoft-graph-api-throws-the-mailbox-is-either-i](https://learn.microsoft.com/en-us/answers/questions/761931/microsoft-graph-api-throws-the-mailbox-is-either-i)

## 在 Flowise 中创建凭据

1. 添加新的 Microsoft Outlook OAuth2 凭据
2. 输入凭据的名称。
3. 复制 OAuth 重定向 URL。
4. 请注意，需要填写以下字段：
   * 授权URL
   * 访问令牌 URL
   * 客户ID
   * 客户秘密

<figure><img src="../../../.gitbook/assets/image (175).png" alt="" width="437"><figcaption></figcaption></figure>

## 创建 Azure 应用程序<a href="#h_8276f6aa94" id="h_8276f6aa94"></a>

1. 登录您现有的[Azure](https://login.microsoftonline.com/)帐户，或者[注册](https://signup.live.com/signup)（如果您尚未注册）
2. 搜索**应用程序注册**。
3. 接下来，在[应用注册](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/CreateApplicationBlade/quickStartType~/null/isMSAApp~/false)中注册一个新的 Azure 应用程序。

<figure><img src="../../../.gitbook/assets/image (192).png" alt="" width="304"><figcaption></figcaption></figure>

4. 在“重定向 URI（可选）”下，选择“Web”并粘贴您之前复制的“OAuth 重定向 URL”。

<figure><img src="../../../.gitbook/assets/image (197).png" alt=""><figcaption></figcaption></figure>

5. 创建应用程序后，导航到 **证书和密钥** > **客户端密钥**，然后单击“**新客户端密钥**”按钮以创建客户端密钥。复制秘密以供稍后使用。

<figure><img src="../../../.gitbook/assets/image (200).png" alt=""><figcaption></figcaption></figure>

6. 导航到**概述**并单击“**端点**”。复制“**OAuth 2.0 授权端点 (v2)**”和“**OAuth 2.0 令牌端点 (v2)**”的端点。

<figure><img src="../../../.gitbook/assets/image (202).png" alt=""><figcaption></figcaption></figure>

7. 关闭端点弹出窗口，复制**应用程序（客户端）ID**：

<figure><img src="../../../.gitbook/assets/image (203).png" alt="" width="563"><figcaption></figcaption></figure>

## 在 Flowise 中完成设置

1. 填写之前复制的所有值。然后点击“**验证**”：

<figure><img src="../../../.gitbook/assets/image (204).png" alt="" width="440"><figcaption></figcaption></figure>

2. 将弹出 Microsoft 窗口，选择帐户。

{% hint style="warning" %}
帐户用户必须拥有有效的 Microsoft 365 许可
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (205).png" alt="" width="459"><figcaption></figcaption></figure>

3. 授予所需的权限：

<figure><img src="../../../.gitbook/assets/image (206).png" alt="" width="454"><figcaption></figcaption></figure>

4. 弹出窗口将自动关闭，随后将保存凭据。

## 用作代理工具

可以选择多种操作，让代理智能地选择合适的操作。\
参数可以留空，以允许代理自行确定值。但是，如果用户提供值，这些值将覆盖代理的选择。

<figure><img src="../../../.gitbook/assets/image (207).png" alt=""><figcaption></figcaption></figure>

## 用作工具节点

它还可以在确定的工作流程场景中用作工具节点。例如，在继续下一步之前检索 Outlook 邮件列表。\
在此模式下，**工具输入参数必须显式定义并填充值**。\
与 [**用作代理工具**](microsoft-outlook.md#use-as-agent-tool) 选项不同，没有代理可以自动确定输入。用户必须手动填充字段，方法是输入固定值或使用双大括号 `{{ }}` 中包含的变量。

<figure><img src="../../../.gitbook/assets/image (208).png" alt=""><figcaption></figcaption></figure>

