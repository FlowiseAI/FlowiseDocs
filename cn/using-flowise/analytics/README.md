---
description: 了解如何分析聊天流和座席流并对其进行故障排除
---

# 解析

***

Flowise 为 [智能体流程 V2](../agentflowv2.md) 提供逐步跟踪：

<figure><img src="../../.gitbook/assets/image (332).png" alt=""><figcaption></figcaption></figure>

此外，Flowise 还集成了多个分析提供商：

* [LunaryAI](https://lunary.ai/)
* [朗史密斯](https://smith.langchain.com/)
* [Langfuse](https://langfuse.com/)
* [LangWatch](https://langwatch.ai/)
* [Arize](https://arize.com/)
* [菲尼克斯](https://phoenix.arize.com/)
* [Opik](https://www.comet.com/site/products/opik/)

## 设置

1. 在 聊天流 或 智能体流程 的右上角，单击 **设置** > **配置**

<figure><img src="../../.gitbook/assets/analytic-1.webp" alt="Screenshot of user clicking in the configuration menu" width="375"><figcaption></figcaption></figure>

2. 然后进入分析聊天流部分

<figure><img src="../../.gitbook/assets/analytic-2.png" alt="Screenshot of the Analyse Chatflow section with the different Analytics providers"><figcaption></figcaption></figure>

3.您将看到提供者列表及其配置字段

<figure><img src="../../.gitbook/assets/image (82).png" alt="Screenshot of an analytics provider with credentials fields expanded"><figcaption></figcaption></figure>

4. 填写凭据和其他配置详细信息，然后打开提供商**开启**。单击“保存”。

<figure><img src="../../.gitbook/assets/image (83).png" alt="Screenshot of analytics providers enabled"><figcaption></figcaption></figure>

## API

从 UI 打开分析后，您可以在 [预测 API](api.md#prediction-api) 主体中覆盖或提供其他配置：

```json
{
  "question": "hi there",
  "overrideConfig": {
    "analytics": {
      "langFuse": {
        // langSmith, langFuse, lunary, langWatch, opik
        "userId": "user1"
      }
    }
  }
}
```
