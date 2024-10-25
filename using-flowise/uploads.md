---
description: Learn how to use upload files/images/audio
---

# Uploads

Flowise allow users to upload files/images/audios from the chat. In this section, we will go through each and see how to enable the feature and using it.

## Image

Certain Chat Models allow user to input images:

* [ChatOpenAI](../integrations/llamaindex/chat-models/chatopenai.md)
* [AzureChatOpenAI](../integrations/llamaindex/chat-models/azurechatopenai.md)
* [ChatAnthropic](../integrations/langchain/chat-models/chatanthropic.md)
* [AWSChatBedrock](../integrations/langchain/chat-models/aws-chatbedrock.md)
* [ChatGoogleGenerativeAI](../integrations/langchain/chat-models/google-ai.md)

When **Allow Image Upload** is enabled, images can be uploaded from chat interface.

<div align="center">

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="255"><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2024-02-29 011714.png" alt="" width="290"><figcaption></figcaption></figure>

</div>

To perform the same using API:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatlfowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Can you describe the image?",
    "uploads": [
        {
            "data": 'data:image/png;base64,iVBORw0KGgdM2uN0', # base64 string or url
            "type": 'file', # file | url
            "name": 'Flowise.png',
            "mime": 'image/png'
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatlfowid>",
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
            "data": 'data:image/png;base64,iVBORw0KGgdM2uN0', //base64 string or url
            "type": 'file', //file | url
            "name": 'Flowise.png',
            "mime": 'image/png'
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## Audio

Under Chatflow Configuration, user can select a **Speech to Text** module. Supported integrations are:

* OpenAI
* AssemblyAI
* [LocalAI](../integrations/langchain/chat-models/chatlocalai.md)

When enabled, users can speak directly into microphone and speech will be transcribed into text.

<div align="left">

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2024-02-29 012538.png" alt="" width="431"><figcaption></figcaption></figure>

</div>

To perform the same using API:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatlfowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "uploads": [
        {
            "data": 'data:audio/webm;codecs=opus;base64,GkXf', #base64 string
            "type": 'audio',
            "name": 'audio.wav',
            "mime": 'audio/webm'
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatlfowid>",
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
            "data": 'data:audio/webm;codecs=opus;base64,GkXf', //base64 string
            "type": 'audio',
            "name": 'audio.wav',
            "mime": 'audio/webm'
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## Files

There are 2 types of file uploads

* RAG File Uploads
* Full File Uploads

When both options are turned on, Full File Uploads will takes precedence.

### RAG File Uploads

Uploaded files will be upserted on the fly to the vector store. However, to enable file uploads feature, there are a few pre-requisites.

* A file-upload supported vector store must be present in the chatflow.
  * [Pinecone](../integrations/langchain/vector-stores/pinecone.md)
  * [Milvus](../integrations/langchain/vector-stores/milvus.md)
  * [Postgres](../integrations/langchain/vector-stores/postgres.md)
  * [Qdrant](../integrations/langchain/vector-stores/qdrant.md)
  * [Upstash](../integrations/langchain/vector-stores/upstash-vector.md)
* If you have multiple vector stores on a chatflow, you can only turn on file upload for one vector store at a time.
* At least one Document Loader node should be connected to the vector store's Document input.
* Supported Document Loader:
  * [CSV File](../integrations/langchain/document-loaders/csv-file.md)
  * [Docx File](../integrations/langchain/document-loaders/docx-file.md)
  * [Json File](../integrations/langchain/document-loaders/json-file.md)
  * [Json Lines File](../integrations/langchain/document-loaders/json-lines-file.md)
  * [PDF File](../integrations/langchain/document-loaders/pdf-file.md)
  * [Text File](../integrations/langchain/document-loaders/text-file.md)
  * [Unstructured File](../integrations/langchain/document-loaders/unstructured-file-loader.md)

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

One or multiple files can be uploaded on the chat:

<div align="left">

<figure><img src="../.gitbook/assets/image (3) (1) (1).png" alt="" width="380"><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2024-08-26 170456.png" alt=""><figcaption></figcaption></figure>

</div>

Here's how it works:

* Uploaded files will have the metadata updated with the chatId.
* This will allow the file to be associated with the chatId.
* When querying, an **OR** filter is being applied:
  * Metadata contains `flowise_chatId` and the value equals to the current chat session id
  * Metadata does not contains `flowise_chatId`

An example of a vector embeddings upserted on Pinecone:

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

To perform the same using API, it is a 2 steps process.

1. [Vector Upsert API](api.md#vector-upsert-api) with `formData` and `chatId`:

{% tabs %}
{% tab title="Python" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatlfowid>"

# use form data to upload files
form_data = {
    "files": ('state_of_the_union.txt', open('state_of_the_union.txt', 'rb'))
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
// use FormData to upload files
let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("chatId", "some-session-id");

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatlfowid>",
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

2. [Prediction API](api.md#prediction) with `uploads` and same `chatId` as step 1

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatlfowid>"

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
        "http://localhost:3000/api/v1/prediction/<chatlfowid>",
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

The downside of RAG File Uploads is that it is incapable of working with structured data such as speadsheets, tables. It also unable to do full summarization as it doesn't have the full context. In some cases, you might want to stuff all the content of the file directly into the prompt for LLM, especially models with longer context window such as Gemini and Claude. There are many [research papers](https://arxiv.org/html/2407.16833v1) around RAG vs Longer Context.

To enable full file upload, go to Chatflow Configuration, File Upload tab, and enable it:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

You will be able to see the File Attachment button from the chat where you can upload one or multiple files. Under the hood, each file is being processed by [File Loader](../integrations/langchain/document-loaders/file-loader.md), and converted into text.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

To perform the same using API:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatlfowid>"

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
        "http://localhost:3000/api/v1/prediction/<chatlfowid>",
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

As seen from the examples, uploads require base64 string. To get base64 string from files, you can use [Create Attachments API](../api-reference/attachments.md).
