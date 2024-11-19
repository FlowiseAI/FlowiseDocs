---
description: Learn how to use upload images, audio, and other files
---

# Uploads

Flowise lets you upload images, audio, and other files from the chat. In this section, you'll learn how to enable and use these features.

## Image

Certain chat models allow you to input images. Always refer to the official documentation of the LLM to confirm if the model supports image input.

* [ChatOpenAI](../integrations/llamaindex/chat-models/chatopenai.md)
* [AzureChatOpenAI](../integrations/llamaindex/chat-models/azurechatopenai.md)
* [ChatAnthropic](../integrations/langchain/chat-models/chatanthropic.md)
* [AWSChatBedrock](../integrations/langchain/chat-models/aws-chatbedrock.md)
* [ChatGoogleGenerativeAI](../integrations/langchain/chat-models/google-ai.md)
* [ChatOllama](../integrations/llamaindex/chat-models/chatollama.md)
* [Google Vertex AI](../integrations/langchain/llms/googlevertex-ai.md)

{% hint style="warning" %}
Image processing only works with certain chains/agents in Chatflow.

[LLMChain](../integrations/langchain/chains/llm-chain.md), [Conversation Chain](../integrations/langchain/chains/conversation-chain.md), [ReAct Agent](../integrations/langchain/agents/react-agent-chat.md), [Conversational Agent](../integrations/langchain/agents/conversational-agent.md), [Tool Agent](../integrations/langchain/agents/tool-agent.md)
{% endhint %}

If you enable **Allow Image Upload**, you can upload images from the chat interface.

<div align="center">

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="255"><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2024-02-29 011714.png" alt="" width="290"><figcaption></figcaption></figure>

</div>

To upload images with the API:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Can you describe the image?",
    "uploads": [
        {
            "data": "data:image/png;base64,iVBORw0KGgdM2uN0", # base64 string or url
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
    "question": "Can you describe the image?",
    "uploads": [
        {
            "data": "data:image/png;base64,iVBORw0KGgdM2uN0", //base64 string or url
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

In the Chatflow Configuration, you can select a speech-to-text module. Supported integrations include:

* OpenAI
* AssemblyAI
* [LocalAI](../integrations/langchain/chat-models/chatlocalai.md)

When this is enabled, users can speak directly into the microphone. Their speech is be transcribed into text.

<div align="left">

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2024-02-29 012538.png" alt="" width="431"><figcaption></figcaption></figure>

</div>

To upload audio with the API:

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
            "data": "data:audio/webm;codecs=opus;base64,GkXf", # base64 string
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
            "data": "data:audio/webm;codecs=opus;base64,GkXf", // base64 string
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

You can upload files in two ways:

* Retrieval augmented generation (RAG) file uploads
* Full file uploads

When both options are on, full file uploads take precedence.

### RAG File Uploads

You can upsert uploaded files on the fly to the vector store. To enable file uploads, make sure you meet these prerequisites:

* You must include a vector store that supports file uploads in the chatflow.
  * [Pinecone](../integrations/langchain/vector-stores/pinecone.md)
  * [Milvus](../integrations/langchain/vector-stores/milvus.md)
  * [Postgres](../integrations/langchain/vector-stores/postgres.md)
  * [Qdrant](../integrations/langchain/vector-stores/qdrant.md)
  * [Upstash](../integrations/langchain/vector-stores/upstash-vector.md)
* If you have multiple vector stores in a chatflow, you can only turn on file upload for one vector store at a time.
* You must connect at least one document loader node to the vector store's document input.
* Supported document loaders:
  * [CSV File](../integrations/langchain/document-loaders/csv-file.md)
  * [Docx File](../integrations/langchain/document-loaders/docx-file.md)
  * [Json File](../integrations/langchain/document-loaders/json-file.md)
  * [Json Lines File](../integrations/langchain/document-loaders/json-lines-file.md)
  * [PDF File](../integrations/langchain/document-loaders/pdf-file.md)
  * [Text File](../integrations/langchain/document-loaders/text-file.md)
  * [Unstructured File](../integrations/langchain/document-loaders/unstructured-file-loader.md)

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

You can upload one or more files in the chat:

<div align="left">

<figure><img src="../.gitbook/assets/image (3) (1) (1).png" alt="" width="380"><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2024-08-26 170456.png" alt=""><figcaption></figcaption></figure>

</div>

Here's how it works:

1. The metadata for uploaded files is updated with the chatId.
2. This associates the file with the chatId.
3. When querying, an **OR** filter applies:

* Metadata contains `flowise_chatId`, and the value is the current chat session ID
* Metadata does not contain `flowise_chatId`

An example of a vector embedding upserted on Pinecone:

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

To do this with the API, follow these two steps:

1. Use the [Vector Upsert API](api.md#vector-upsert-api) with `formData` and `chatId`:

{% tabs %}
{% tab title="Python" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowid>"

# Use form data to upload files
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
// Use FormData to upload files
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

2. Use the [Prediction API](api.md#prediction) with `uploads` and the `chatId` from step 1:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "What is the speech about?",
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
    "question": "What is the speech about?",
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

With RAG file uploads, you can't work with structured data like spreadsheets or tables, and you can't perform full summarization due to lack of full context. In some cases, you might want to include all the file content directly in the prompt for an LLM, especially with models like Gemini and Claude that have longer context windows. [This research paper](https://arxiv.org/html/2407.16833v1) is one of many that compare RAG with longer context windows.

To enable full file uploads, go to **Chatflow Configuration**, open the **File Upload** tab, and click the switch:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

You can see the **File Attachment** button in the chat, where you can upload one or more files. Under the hood, the [File Loader](../integrations/langchain/document-loaders/file-loader.md) processes each file and converts it into text.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

To upload files with the API:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "What is the data about?",
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
    "question": "What is the data about?",
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

As you can see in the examples, uploads require a base64 string. To get a base64 string for a file, use the [Create Attachments API](../api-reference/attachments.md).
