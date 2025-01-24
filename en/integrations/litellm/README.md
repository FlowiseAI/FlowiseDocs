---
description: Learn how Flowise integrates with LiteLLM Proxy
---

# LiteLLM Proxy

Use [LiteLLM Proxy](https://docs.litellm.ai/docs/simple_proxy) with Flowise to:

- Load balance Azure OpenAI/LLM endpoints
- Call 100+ LLMs in the OpenAI Format 
- Use Virtual Keys to set budgets, rate limits and track usage

## How to use LiteLLM Proxy with Flowise

### Step 1: Define your LLM Models in the LiteLLM config.yaml file

LiteLLM Requires a config with all your models defined - we will call this file `litellm_config.yaml`

[Detailed docs on how to setup litellm config - here](https://docs.litellm.ai/docs/proxy/configs)

```yaml
model_list:
  - model_name: gpt-4
    litellm_params:
      model: azure/chatgpt-v-2
      api_base: https://openai-gpt-4-test-v-1.openai.azure.com/
      api_version: "2023-05-15"
      api_key: 
  - model_name: gpt-4
    litellm_params:
      model: azure/gpt-4
      api_key: 
      api_base: https://openai-gpt-4-test-v-2.openai.azure.com/
  - model_name: gpt-4
    litellm_params:
      model: azure/gpt-4
      api_key: 
      api_base: https://openai-gpt-4-test-v-2.openai.azure.com/
```


### Step 2. Start litellm proxy

```shell
docker run \
    -v $(pwd)/litellm_config.yaml:/app/config.yaml \
    -p 4000:4000 \
    ghcr.io/berriai/litellm:main-latest \
    --config /app/config.yaml --detailed_debug
```

On success, the proxy will start running on `http://localhost:4000/`

### Step 3: Use the LiteLLM Proxy in Flowise

In Flowise, specify the **standard OpenAI nodes (not the Azure OpenAI nodes)** -- this goes for **chat models, embeddings, llms -- everything**

- Set `BasePath` to LiteLLM Proxy URL (`http://localhost:4000` when running locally)
- Set the following headers `Authorization: Bearer <your-litellm-master-key>`

