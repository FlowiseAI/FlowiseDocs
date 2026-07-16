# RAG

大型语言模型 (LLM) 释放了创建高级问答聊天机器人的潜力，能够根据特定内容提供精确的答案。这些系统依赖于一种称为检索增强生成（RAG）的方法，该方法通过将它们扎根于相关源材料来增强它们的响应。

在本教程中，您将学习如何创建一个基本的问答应用程序，该应用程序可以从给定的文档源中提取和回答问题。

该流程可以分为 2 个子流程：

* 索引
* 检索

## 索引

[文档存储](../using-flowise/document-stores.md) 旨在帮助完成整个索引管道 - 从不同来源检索数据、分块策略、写入更新到矢量数据库、与更新的数据同步。

我们支持广泛的文档加载器，从 Pdf、Word、Google Drive 等文件到 Playwright、Firecrawl、Apify 等网络抓取工具。您还可以创建自定义文档加载器。

<figure><img src="../.gitbook/assets/image (293).png" alt="" width="563"><figcaption></figcaption></figure>

## 检索

根据用户的输入，从向量数据库中获取相关文档块。 LLM 然后使用检索到的上下文来生成响应。

1. 拖放 [Agent](../using-flowise/agentflowv2.md#id-3.-agent-node) 节点，并配置要使用的模型。

<figure><img src="../.gitbook/assets/image (290).png" alt="" width="391"><figcaption></figcaption></figure>

2. 添加新的知识（文档存储）并定义内容的内容。这有助于 LLM 了解何时以及如何检索相关信息。您还可以使用自动生成按钮来协助完成此过程。

{% hint style="success" %}
只能使用写入更新的文档存储
{% endhint %}

<figure><img src="../.gitbook/assets/image (288).png" alt="" width="482"><figcaption></figcaption></figure>

3.（可选）如果数据已经存储在矢量数据库中，而无需经过文档存储索引管道，您还可以直接连接到矢量数据库和嵌入模型。

<figure><img src="../.gitbook/assets/image (289).png" alt="" width="388"><figcaption></figcaption></figure>

4. 添加系统提示词，或使用**生成**按钮来协助。我们建议使用它，因为它有助于制作更有效和优化的提示词。

<figure><img src="../.gitbook/assets/image (294).png" alt="" width="482"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (292).png" alt="" width="563"><figcaption></figcaption></figure>

5. 您的 RAG 代理现在可以使用了！

## 资源

{% embed url="https://youtu.be/KHc0ClOIv0A?si=mEZJydM8bT2imKJY" %}
