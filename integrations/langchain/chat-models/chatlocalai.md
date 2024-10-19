# ChatLocalAI

## LocalAI Setup

[**LocalAI** ](https://github.com/go-skynet/LocalAI)is a drop-in replacement REST API that’s compatible with OpenAI API specifications for local inferencing. It allows you to run LLMs (and not only) locally or on-prem with consumer grade hardware, supporting multiple model families that are compatible with the ggml format.

To use ChatLocalAI within Flowise, follow the steps below:

1. ```bash
   git clone https://github.com/go-skynet/LocalAI
   ```
2. <pre class="language-bash"><code class="lang-bash"><strong>cd LocalAI
   </strong></code></pre>
3. ```bash
   # copy your models to models/
   cp your-model.bin models/
   ```

For example:

Download one of the models from [gpt4all.io](https://gpt4all.io/index.html)

```bash
# Download gpt4all-j to models/
wget https://gpt4all.io/models/ggml-gpt4all-j.bin -O models/ggml-gpt4all-j
```

In the `/models` folder, you should be able to see the downloaded model in there:

<figure><img src="../../../.gitbook/assets/image (22) (1).png" alt=""><figcaption></figcaption></figure>

Refer [here](https://localai.io/model-compatibility/index.html) for list of supported models.

4. ```bash
   docker compose up -d --pull always
   ```
5. Now API is accessible at localhost:8080

```bash
# Test API
curl http://localhost:8080/v1/models
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

That's it! For more information, refer to LocalAI [docs](https://localai.io/basics/getting\_started/index.html).

Watch how you can use LocalAI on Flowise

{% embed url="https://youtu.be/0B0oIs8NS9k" %}
