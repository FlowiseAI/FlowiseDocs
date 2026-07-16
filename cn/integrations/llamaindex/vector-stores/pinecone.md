---
description: >-
  使用 Pinecone 写入更新嵌入数据并根据查询执行相似性搜索，
  领先的完全管理的托管矢量数据库。
---

# 松果

## 先决条件

1. 注册 [Pinecone](https://app.pinecone.io/) 帐户
2. 点击**创建索引**

<figure><img src="../../../.gitbook/assets/pinecone_1.png" alt=""><figcaption></figcaption></figure>

3. 填写必填字段：
* **索引名称**，要创建的索引的名称。 （例如“流程测试”）
   * **维度**，要插入索引中的向量的大小。 （例如 1536）

<figure><img src="../../../.gitbook/assets/pinecone_2.png" alt="" width="527"><figcaption></figcaption></figure>

4. 单击**创建索引**

## 设置

1. 获取/创建您的 **API 密钥**

<figure><img src="../../../.gitbook/assets/pinecone_3.png" alt=""><figcaption></figcaption></figure>

2. 在画布上添加一个新的 **Pinecone** 节点并填写参数：
   * 松果指数
   * Pinecone命名空间（可选）

<figure><img src="../../../.gitbook/assets/pinecone_llamaindex.png" alt="" width="301"><figcaption><p>松果节点</p></figcaption></figure>

3. 创建新的 Pinecone 凭据 -> 填写 **API 密钥**

<figure><img src="../../../.gitbook/assets/pinecone_5.png" alt="" width="563"><figcaption></figcaption></figure>

4. 将其他节点添加到画布并启动写入更新过程
   * **文档**可以与[**文档加载器**](../../langchain/document-loaders/)类别下的任何节点连接

       <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p>LlamaIndex 的文档加载器和文本分割器尚不可用，但使用 LangChain 下可用的文档加载器和文本分割器之一仍允许正常使用 LlamaIndex 进行查询。</p></div>

\- \*\*Embeddings\*\* 可以与 \[\*\*Embeddings\*\* ]\(../embeddings/)category 下的任意节点连接

<figure><img src="../../../.gitbook/assets/pinecone_llama_chatflow.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/pinecone_llama_upsert.png" alt=""><figcaption></figcaption></figure>

5. 在 [Pinecone 仪表板](https://app.pinecone.io) 上验证数据是否已成功写入更新：

<figure><img src="../../../.gitbook/assets/pinecone_8.png" alt=""><figcaption></figcaption></figure>
