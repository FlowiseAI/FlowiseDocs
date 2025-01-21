# ChatOpenAI

## Prerequisitos

1. Una cuenta de [OpenAI](https://openai.com/)
2. Crear una [API key](https://platform.openai.com/api-keys)

## Configuración

1. **Chat Models** > arrastra el nodo **ChatOpenAI**

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. **Connect Credential** > haz clic en **Create New**

<figure><img src="../../../.gitbook/assets/image_openAI (1).png" alt="" width="278"><figcaption></figcaption></figure>

3. Completa la credencial de **ChatOpenAI**

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

4. ¡Voilà [🎉](https://emojipedia.org/party-popper/), ahora puedes usar el **nodo ChatOpenAI** en Flowise

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## URL base y encabezados personalizados

Flowise admite el uso de URL base y encabezados personalizados para Chat OpenAI. Los usuarios pueden usar fácilmente integraciones como OpenRouter, TogetherAI y otros que sean compatibles con la API de OpenAI.

### TogetherAI

1. Consulta la [documentación oficial](https://docs.together.ai/docs/openai-api-compatibility#nodejs) de TogetherAI
2. Crea una nueva credencial con la API key de TogetherAI
3. Haz clic en **Additional Parameters** en el nodo ChatOpenAI
4. Cambia el Base Path:

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

### Open Router

1. Consulta la [documentación oficial](https://openrouter.ai/docs#quick-start) de OpenRouter
2. Crea una nueva credencial con la API key de OpenRouter
3. Haz clic en Additional Parameters en el nodo ChatOpenAI
4. Cambia el Base Path y Base Options:

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## Modelo Personalizado

Para modelos que no están soportados en el nodo ChatOpenAI, puedes usar ChatOpenAI Custom. Esto permite a los usuarios introducir nombres de modelos como `mistralai/Mixtral-8x7B-Instruct-v0.1`

<figure><img src="../../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

## Subida de Imágenes

También puedes permitir que las imágenes sean subidas y analizadas por el LLM. Internamente, Flowise utilizará el modelo [OpenAI Vision](https://platform.openai.com/docs/guides/vision) para procesar la imagen. Solo funciona con LLMChain, Conversation Chain, ReAct Agent y Conversational Agent.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="332"><figcaption></figcaption></figure>

Desde la interfaz de chat, ahora verás un nuevo botón de subida de imágenes:

<figure><img src="../../../.gitbook/assets/Untitled (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>
