---
description: 了解 Flowise 如何与 LiteLLM Proxy 集成
---

# LiteLLM 代理

将 [LiteLLM 代理](https://docs.litellm.ai/docs/simple_proxy) 与 Flowise 结合使用，可以：

- 负载平衡 Azure OpenAI/LLM 端点
- 调用 OpenAI 格式的 100 多个法学硕士 
- 使用虚拟钥匙设置预算、速率限制并跟踪使用情况

## 如何将 LiteLLM 代理与 Flowise 结合使用

### 步骤 1：在 LiteLLM config.yaml 文件中定义 LLM 模型

LiteLLM 需要定义所有模型的配置 - 我们将此文件称为 `litellm_config.yaml`

[有关如何设置 litellm 配置的详细文档 - 此处](https://docs.litellm.ai/docs/proxy/configs)

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


### 步骤 2. 启动 litellm 代理

```shell
docker run \
    -v $(pwd)/litellm_config.yaml:/app/config.yaml \
    -p 4000:4000 \
    ghcr.io/berriai/litellm:main-latest \
    --config /app/config.yaml --detailed_debug
```

成功后，代理将开始在 `http://localhost:4000/` 上运行

### 步骤 3：在 Flowise 中使用 LiteLLM 代理

在 Flowise 中，指定 **标准 OpenAI 节点（不是 Azure OpenAI 节点）** - 这适用于 **聊天模型、嵌入、llms - 一切**

- 将 `BasePath` 设置为 LiteLLM 代理 URL（本地运行时为 `http://localhost:4000`）
- 设置以下标头“授权：承载者”<your-litellm-master-key>`

