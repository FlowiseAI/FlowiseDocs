# LocalAI 嵌入

## LocalAI 设置

[**LocalAI**](https://github.com/go-skynet/LocalAI) 是 REST API 的直接替代品，与 OpenAI API 本地推理规范兼容。它允许您使用消费级硬件在本地或本地运行 LLM（不仅如此），支持与 ggml 格式兼容的多个模型系列。

要在 Flowise 中使用 LocalAI Embeddings，请按照以下步骤操作：

1. ```bash
   git 克隆 https://github.com/go-skynet/LocalAI
   ```
2. <pre class="language-bash"><code class="lang-bash"><strong>cd LocalAI
   </strong></code></pre>
3. LocalAI provides an [API endpoint](https://localai.io/api-endpoints/index.html#applying-a-model---modelsapply) to download/install the model. In this example, we are going to use BERT Embeddings model:

<figure><img src="../../../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>

4. In the `/models` folder, you should be able to see the downloaded model in there:

<figure><img src="../../../.gitbook/assets/image (23) (1) (2).png" alt=""><figcaption></figcaption></figure>

5. You can now test the embeddings:

```bash
卷曲 http://localhost:8080/v1/embeddings -H“内容类型：application/json”-d '{
    “输入”：“测试”，
    “模型”：“文本嵌入-ada-002”
  }'
```

6. Response should looks like:

<figure><img src="../../../.gitbook/assets/image (29).png" alt="" width="375"><figcaption></figcaption></figure>

## Flowise Setup

Drag and drop a new LocalAIEmbeddings component to canvas:

<figure><img src="../../../.gitbook/assets/image (21) (1) (2).png" alt=""><figcaption></figcaption></figure>

Fill in the fields:

* **Base Path**: The base url from LocalAI such as [http://localhost:8080/v1](http://localhost:8080/v1)
* **Model Name**: The model you want to use. Note that it must be inside `/models` folder of LocalAI directory. For instance: `text-embedding-ada-002`

That's it! For more information, refer to LocalAI [docs](https://localai.io/models/index.html#embeddings-bert).
