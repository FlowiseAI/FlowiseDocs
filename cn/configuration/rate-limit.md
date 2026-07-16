---
description: 了解如何在 Flowise 中管理 API 请求
---

# 速率限制

***

当您在没有 API 授权的情况下通过 API 或嵌入式聊天向公众共享聊天流时，任何人都可以访问该流。为了防止垃圾邮件，您可以对聊天流设置速率限制。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="462"><figcaption></figcaption></figure>

* **每个持续时间的消息限制**：在特定持续时间内可以接收多少条消息。例如：20
* **持续时间（以秒为单位）**：指定的持续时间。例如：60
* **限制消息**：超过限制时返回什么消息。例如：超出配额

使用上面的示例，这意味着 60 秒内只允许接收 20 条消息。速率限制由 IP 地址跟踪。如果您已在云服务上部署 Flowise，则必须设置 `NUMBER_OF_PROXIES` 环境变量。

## 速率限制设置

当您在云上托管 Flowise（例如 AWS、GCP、Azure 等）时，您很可能位于代理/负载均衡器后面。因此，速率限制可能无法发挥作用。如需了解更多信息，请访问[此处](https://github.com/express-rate-limit/express-rate-limit/wiki/Troubleshooting-Proxy-Issues)。

要解决该问题：

1. **设置环境变量：** 在您的托管环境中创建名为 `NUMBER_OF_PROXIES` 的环境变量并将其值设置为 `0`。
2. **重新启动托管的 Flowise 实例：** 这使 Flowise 能够应用环境变量的更改。
3. **检查 IP 地址：** 要验证 IP 地址，请访问以下 URL：`{{hosted_url}}/api/v1/ip`。您可以通过在网络浏览器中输入 URL 或发出 API 请求来执行此操作。
4. **比较IP地址** 发出请求后，将返回的IP地址与您当前的IP地址进行比较。您可以通过访问以下任一网站找到您当前的 IP 地址：
   * [http://ip.nfriedly.com/](http://ip.nfriedly.com/)
   * [https://api.ipify.org/](https://api.ipify.org/)
5. **IP 地址不正确：** 如果返回的 IP 地址与您当前的 IP 地址不匹配，请将 `NUMBER_OF_PROXIES` 加 1 并重新启动 Flowise 实例。重复此过程，直到 IP 地址与您的 IP 地址匹配。
