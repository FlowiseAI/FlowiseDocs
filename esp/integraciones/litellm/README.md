---
description: Aprende cómo Flowise se integra con LiteLLM Proxy
---

# LiteLLM Proxy

Usa [LiteLLM Proxy](https://docs.litellm.ai/docs/simple_proxy) con Flowise para:

- Balancear la carga de endpoints de Azure OpenAI/LLM
- Llamar a más de 100 LLMs en el formato OpenAI 
- Usar Claves Virtuales para establecer presupuestos, límites de tasa y rastrear el uso

## Cómo usar LiteLLM Proxy con Flowise

### Paso 1: Define tus modelos LLM en el archivo config.yaml de LiteLLM

LiteLLM requiere una configuración con todos tus modelos definidos - llamaremos a este archivo `litellm_config.yaml`

[Documentación detallada sobre cómo configurar litellm config - aquí](https://docs.litellm.ai/docs/proxy/configs)

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


### Paso 2. Inicia el proxy litellm

```shell
docker run \
    -v $(pwd)/litellm_config.yaml:/app/config.yaml \
    -p 4000:4000 \
    ghcr.io/berriai/litellm:main-latest \
    --config /app/config.yaml --detailed_debug
```

Si tiene éxito, el proxy comenzará a ejecutarse en `http://localhost:4000/`

### Paso 3: Usa el LiteLLM Proxy en Flowise

En Flowise, especifica los **nodos estándar de OpenAI (no los nodos de Azure OpenAI)** -- esto aplica para **modelos de chat, embeddings, llms -- todo**

- Establece `BasePath` a la URL del LiteLLM Proxy (`http://localhost:4000` cuando se ejecuta localmente)
- Establece los siguientes headers `Authorization: Bearer <tu-litellm-master-key>`

