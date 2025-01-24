# LocalAI Embeddings

## LocalAI Setup

[**LocalAI** ](https://github.com/go-skynet/LocalAI)is a drop-in replacement REST API that’s compatible with OpenAI API specifications for local inferencing. It allows you to run LLMs (and not only) locally or on-prem with consumer grade hardware, supporting multiple model families that are compatible with the ggml format.

To use LocalAI Embeddings within Flowise, follow the steps below:

1. ```bash
   git clone https://github.com/go-skynet/LocalAI
   ```
2. <pre class="language-bash"><code class="lang-bash"><strong>cd LocalAI
   </strong></code></pre>
3. LocalAI provides an [API endpoint](https://localai.io/api-endpoints/index.html#applying-a-model---modelsapply) to download/install the model. In this example, we are going to use BERT Embeddings model:

<figure><img src="../../../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>

4. In the `/models` folder, you should be able to see the downloaded model in there:

<figure><img src="../../../.gitbook/assets/image (23) (1).png" alt=""><figcaption></figcaption></figure>

5. You can now test the embeddings:

```bash
curl http://localhost:8080/v1/embeddings -H "Content-Type: application/json" -d '{
    "input": "Test",
    "model": "text-embedding-ada-002"
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
