---
description: Aprende cómo subir imágenes, audio y otros archivos
---

# Uploads

Flowise te permite subir imágenes, audio y otros archivos desde el chat. En esta sección, aprenderás cómo habilitar y usar estas funcionalidades.

## Image

Ciertos modelos de chat permiten ingresar imágenes. Siempre consulta la documentación oficial del LLM para confirmar si el modelo admite la entrada de imágenes.

* [ChatOpenAI](../integrations/llamaindex/chat-models/chatopenai.md)
* [AzureChatOpenAI](../integrations/llamaindex/chat-models/azurechatopenai.md)
* [ChatAnthropic](../integrations/langchain/chat-models/chatanthropic.md)
* [AWSChatBedrock](../integrations/langchain/chat-models/aws-chatbedrock.md)
* [ChatGoogleGenerativeAI](../integrations/langchain/chat-models/google-ai.md)
* [ChatOllama](../integrations/llamaindex/chat-models/chatollama.md)
* [Google Vertex AI](../integrations/langchain/llms/googlevertex-ai.md)

{% hint style="warning" %}
El procesamiento de imágenes solo funciona con ciertas cadenas/agentes en Chatflow.

[LLMChain](../integrations/langchain/chains/llm-chain.md), [Conversation Chain](../integrations/langchain/chains/conversation-chain.md), [ReAct Agent](../integrations/langchain/agents/react-agent-chat.md), [Conversational Agent](../integrations/langchain/agents/conversational-agent.md), [Tool Agent](../integrations/langchain/agents/tool-agent.md)
{% endhint %}

Si habilitas **Allow Image Upload**, podrás subir imágenes desde la interfaz de chat.

<div align="center"><figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="255"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 011714.png" alt="" width="290"><figcaption></figcaption></figure></div>

Para subir imágenes con la API:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "¿Puedes describir la imagen?",
    "uploads": [
        {
            "data": "data:image/png;base64,iVBORw0KGgdM2uN0", # cadena base64 o url
            "type": "file", # file | url
            "name": "Flowise.png",
            "mime": "image/png"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "¿Puedes describir la imagen?",
    "uploads": [
        {
            "data": "data:image/png;base64,iVBORw0KGgdM2uN0", //cadena base64 o url
            "type": "file", // file | url
            "name": "Flowise.png",
            "mime": "image/png"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## Audio

En la Configuración del Chatflow, puedes seleccionar un módulo de speech-to-text. Las integraciones soportadas incluyen:

* OpenAI
* AssemblyAI
* [LocalAI](../integrations/langchain/chat-models/chatlocalai.md)

Cuando esto está habilitado, los usuarios pueden hablar directamente al micrófono. Su voz se transcribe a texto.

<div align="left"><figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 012538.png" alt="" width="431"><figcaption></figcaption></figure></div>

Para subir audio con la API:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "uploads": [
        {
            "data": "data:audio/webm;codecs=opus;base64,GkXf", # cadena base64
            "type": "audio",
            "name": "audio.wav",
            "mime": "audio/webm"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "uploads": [
        {
            "data": "data:audio/webm;codecs=opus;base64,GkXf", // cadena base64
            "type": "audio",
            "name": "audio.wav",
            "mime": "audio/webm"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## Files

Puedes subir archivos de dos maneras:

* Carga de archivos para generación aumentada por recuperación (RAG)
* Carga completa de archivos

Cuando ambas opciones están activadas, la carga completa de archivos tiene prioridad.

### RAG File Uploads

Puedes hacer upsert de los archivos subidos al vector store sobre la marcha. Para habilitar la carga de archivos, asegúrate de cumplir con estos requisitos previos:

* Debes incluir un vector store que admita la carga de archivos en el chatflow.
  * [Pinecone](../integrations/langchain/vector-stores/pinecone.md)
  * [Milvus](../integrations/langchain/vector-stores/milvus.md)
  * [Postgres](../integrations/langchain/vector-stores/postgres.md)
  * [Qdrant](../integrations/langchain/vector-stores/qdrant.md)
  * [Upstash](../integrations/langchain/vector-stores/upstash-vector.md)
* Si tienes múltiples vector stores en un chatflow, solo puedes activar la carga de archivos para un vector store a la vez.
* Debes conectar al menos un nodo document loader a la entrada de documentos del vector store.
* Document loaders soportados:
  * [CSV File](../integrations/langchain/document-loaders/csv-file.md)
  * [Docx File](../integrations/langchain/document-loaders/docx-file.md)
  * [Json File](../integrations/langchain/document-loaders/json-file.md)
  * [Json Lines File](../integrations/langchain/document-loaders/json-lines-file.md)
  * [PDF File](../integrations/langchain/document-loaders/pdf-file.md)
  * [Text File](../integrations/langchain/document-loaders/text-file.md)
  * [Unstructured File](../integrations/langchain/document-loaders/unstructured-file-loader.md)

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Puedes subir uno o más archivos en el chat:

<div align="left"><figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt="" width="380"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-08-26 170456.png" alt=""><figcaption></figcaption></figure></div>

Así es como funciona:

1. Los metadatos de los archivos subidos se actualizan con el chatId.
2. Esto asocia el archivo con el chatId.
3. Al consultar, se aplica un filtro **OR**:

* Los metadatos contienen `flowise_chatId`, y el valor es el ID de la sesión de chat actual
* Los metadatos no contienen `flowise_chatId`

Un ejemplo de un vector embedding con upsert en Pinecone:

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

Para hacer esto con la API, sigue estos dos pasos:

1. Usa la [Vector Upsert API](api.md#vector-upsert-api) con `formData` y `chatId`:

{% tabs %}
{% tab title="Python" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowid>"

# Usa form data para subir archivos
form_data = {
    "files": ("state_of_the_union.txt", open("state_of_the_union.txt", "rb"))
}

body_data = {
    "chatId": "some-session-id"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
// Usa FormData para subir archivos
let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("chatId", "some-session-id");

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowid>",
        {
            method: "POST",
            body: formData
        }
    );
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

2. Usa la [Prediction API](api.md#prediction) con `uploads` y el `chatId` del paso 1:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "¿De qué trata el discurso?",
    "chatId": "same-session-id-from-step-1",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:rag",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "¿De qué trata el discurso?",
    "chatId": "same-session-id-from-step-1",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:rag",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Full File Uploads

Con las cargas de archivos RAG, no puedes trabajar con datos estructurados como hojas de cálculo o tablas, y no puedes realizar resúmenes completos debido a la falta de contexto completo. En algunos casos, es posible que desees incluir todo el contenido del archivo directamente en el prompt para un LLM, especialmente con modelos como Gemini y Claude que tienen ventanas de contexto más largas. [Este artículo de investigación](https://arxiv.org/html/2407.16833v1) es uno de muchos que comparan RAG con ventanas de contexto más largas.

Para habilitar la carga completa de archivos, ve a **Chatflow Configuration**, abre la pestaña **File Upload**, y activa el interruptor:

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Puedes ver el botón **File Attachment** en el chat, donde puedes subir uno o más archivos. Internamente, el [File Loader](../integrations/langchain/document-loaders/file-loader.md) procesa cada archivo y lo convierte en texto.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Para subir archivos con la API:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "¿De qué tratan los datos?",
    "chatId": "some-session-id",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:full",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "¿De qué tratan los datos?",
    "chatId": "some-session-id",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:full",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

Como puedes ver en los ejemplos, las cargas requieren una cadena base64. Para obtener una cadena base64 para un archivo, usa la [Create Attachments API](../api-reference/attachments.md).
