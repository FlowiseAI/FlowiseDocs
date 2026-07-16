---
description: 了解如何为 Flowise 实例设置聊天流级访问控制
---

# 流量

***

构建聊天流/智能体流程后，默认情况下，您的流可供公众使用。有权访问 聊天流 ID 的任何人都可以通过 嵌入 或 API 运行预测。

如果您可能希望允许某些人能够访问它并与之交互，您可以通过为该特定聊天流分配 API 键来实现。

## API 键

在仪表板中，导航到 API 密钥 部分，您应该能够看到创建的 DefaultKey。您还可以添加或删除任何键。

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 聊天流

导航到聊天流，现在您可以选择要用于保护聊天流的 API 密钥。

<figure><img src="../../.gitbook/assets/image (16) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

分配 API 密钥后，只有在授权标头提供了在 HTTP 调用期间指定的正确 API 密钥时，才能访问聊天流 API。

```json
"Authorization": "Bearer <your-api-key>"
```

使用 POSTMAN 调用 API 的示例

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
