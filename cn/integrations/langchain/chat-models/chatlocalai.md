# 聊天本地人工智能

## LocalAI 设置

[**LocalAI**](https://github.com/go-skynet/LocalAI) 是 REST API 的直接替代品，与 OpenAI API 本地推理规范兼容。它允许您使用消费级硬件在本地或本地运行 LLM（不仅如此），支持与 ggml 格式兼容的多个模型系列。

要在 Flowise 中使用 ChatLocalAI，请按照以下步骤操作：

1. ```bash
   git 克隆 https://github.com/go-skynet/LocalAI
   ```
2. <pre class="language-bash"><code class="lang-bash"><strong>cd LocalAI
   </strong></code></pre>
3. ```bash
   # copy your models to models/
   cp your-model.bin models/
   ```

例如：

从 [gpt4all.io](https://gpt4all.io/index.html) 下载模型之一

```bash
# Download gpt4all-j to models/
wget https://gpt4all.io/models/ggml-gpt4all-j.bin -O models/ggml-gpt4all-j
```

在 `/models` 文件夹中，您应该能够在其中看到下载的模型：

<figure><img src="../../../.gitbook/assets/image (22) (1) (1).png" alt=""><figcaption></figcaption></figure>

请参阅[此处](https://localai.io/model-compatibility/index.html)，了解支持的型号列表。

4. ```bash
   docker compose up -d --pull 始终
   ```
5. Now API is accessible at localhost:8080

```bash
# 测试 API
卷曲 http://localhost:8080/v1/models
# {"object":"list","data":[{"id":"ggml-gpt4all-j.bin","object":"model"}]}
```

## Flowise Setup

Drag and drop a new ChatLocalAI component to canvas:

<figure><img src="../../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

Fill in the fields:

* **Base Path**: The base url from LocalAI such as [http://localhost:8080/v1](http://localhost:8080/v1)
* **Model Name**: The model you want to use. Note that it must be inside `/models` folder of LocalAI directory. For instance: `ggml-gpt4all-j.bin`

{% hint style="info" %}
If you are running both Flowise and LocalAI on Docker, you might need to change the base path to [http://host.docker.internal:8080/v1](http://host.docker.internal:8080/v1). For Linux based systems the default docker gateway should be used since host.docker.internal is not available: [http://172.17.0.1:8080/v1](http://172.17.0.1:8080/v1)
{% endhint %}

That's it! For more information, refer to LocalAI [docs](https://localai.io/basics/getting_started/index.html).

Watch how you can use LocalAI on Flowise

{% embed url="https://youtu.be/0B0oIs8NS9k" %}
