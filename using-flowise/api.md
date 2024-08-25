---
description: >-
  Learn how to use the some of the most used APIs: prediction, vector-upsert and
  message API
---

# API

Refer to [API Reference](../api-reference.md) for full list of public APIs

## Authentication

The Authorization header must be provided with the correct API key specified during a HTTP call.

```json
"Authorization": "Bearer <your-api-key>"
```

## 1. Prediction API

* POST `/api/v1/prediction/{your-chatflowid}`

Request Body

<table><thead><tr><th width="192">Key</th><th width="347">Description</th><th>Type</th><th>Required</th></tr></thead><tbody><tr><td>question</td><td>User's question</td><td>string</td><td>Yes</td></tr><tr><td>overrideConfig</td><td>Override existing flow configuration</td><td>object</td><td>No</td></tr><tr><td>history</td><td>Prepend history messages at the start of conversation</td><td>array</td><td>No</td></tr></tbody></table>

You can use the chatflow as API and connect to frontend applications.

<figure><img src="../.gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>

### Override Config

You also have the flexibility to override input configuration with **overrideConfig** property.

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/&#x3C;chatlfowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123",
        "returnSourceDocuments": true
    }
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
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123",
        "returnSourceDocuments": true
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### History

You can prepend history messages to give some context to LLM. For example, if you want the LLM to remember user's name:

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/&#x3C;chatlfowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hey, how are you?",
    "history": [
        {
            "role": "apiMessage",
            "content": "Hello how can I help?"
        },
        {
            "role": "userMessage",
            "content": "Hi my name is Brian"
        },
        {
            "role": "apiMessage",
            "content": "Hi Brian, how can I help?"
        },
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
    "question": "Hey, how are you?",
    "history": [
        {
            "role": "apiMessage",
            "content": "Hello how can I help?"
        },
        {
            "role": "userMessage",
            "content": "Hi my name is Brian"
        },
        {
            "role": "apiMessage",
            "content": "Hi Brian, how can I help?"
        },
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Persists Memory

If the chatflow contains [Memory](../integrations/langchain/memory/#memory-nodes) nodes, you can pass a `sessionId` to persists the state of the conversation, so the every subsequent API calls will have context about previous conversation. Otherwise, a new session will be generated each time.

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/&#x3C;chatlfowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123"
    } 
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
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123"
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Variables

You can pass variable in the API to be used by the nodes in the flow. See more: [Variables](api.md#variables)

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/&#x3C;chatlfowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "vars": {
            "foo": "bar"
        }
    }
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
    "question": "Hey, how are you?",
    "overrideConfig": {
        "vars": {
            "foo": "bar"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Image Uploads

When **Allow Image Upload** is enabled, images can be uploaded from chat interface.

<div align="center">

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="255"><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2024-02-29 011714.png" alt="" width="290"><figcaption></figcaption></figure>

</div>

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/&#x3C;chatlfowid>"

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

### Speech to Text

When **Speech to Text** is enabled, users can speak directly into microphone and speech will be transcribed into text.

<div align="left">

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2024-02-29 012538.png" alt="" width="431"><figcaption></figcaption></figure>

</div>

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/&#x3C;chatlfowid>"

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

### Authentication

You can assign/unassign an API key to the chatflow from the UI. Refer [chatflow-level.md](../configuration/authorization/chatflow-level.md "mention") for more details.

## 2. Vector Upsert API

* POST `/api/v1/vector/upsert/{your-chatflowid}`

Request Body

<table><thead><tr><th width="192">Key</th><th width="559">Description</th><th>Type</th><th>Required</th></tr></thead><tbody><tr><td>overrideConfig</td><td>Override existing flow configuration</td><td>object</td><td>No</td></tr><tr><td>stopNodeId</td><td>Node ID of the vector store. When you have multiple vector stores in a flow, you might not want to upsert all of them. Specifying <code>stopNodeId</code> will ensure only that specific vector store node is upserted.</td><td>array</td><td>No</td></tr></tbody></table>

### Document Loaders with Upload

Some document loaders in Flowise allow user to upload files:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

If the flow contains [Document Loaders](../integrations/langchain/document-loaders/) with Upload File functionality, the API looks slightly different. Instead of passing body as **JSON**, **form-data** is being used. This allows you to upload any files to the API.

{% hint style="info" %}
It is user's responsibility to make sure the file type is compatible with the expected file type from document loader. For example, if a Text File Loader is being used, you should only upload file with `.txt` extension.
{% endhint %}

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
    "returnSourceDocuments": True
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
formData.append("returnSourceDocuments", true);

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

### Document Loaders without Upload

For other [Document Loaders](../integrations/langchain/document-loaders/) nodes without Upload File functionality, the API body is in **JSON** format similar to [Prediction API](api.md#prediction-api).

{% tabs %}
{% tab title="Python" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatlfowid>"

def query(form_data):
    response = requests.post(API_URL, json=payload)
    print(response)
    return response.json()

output = query({
    "overrideConfig": { # optional
        "returnSourceDocuments": true
    }
})
print(output)
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatlfowid>",
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
    "overrideConfig": { // optional
        "returnSourceDocuments": true
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Authentication

You can assign/unassign an API key to the vector upsert API from the UI. Refer [chatflow-level.md](../configuration/authorization/chatflow-level.md "mention") for more details.

## 3. Message API

* GET `/api/v1/chatmessage/{your-chatflowid}`
* DELETE `/api/v1/chatmessage/{your-chatflowid}`

Query Parameters

| Param     | Type   | Value       |
| --------- | ------ | ----------- |
| sessionId | string |             |
| sort      | enum   | ASC or DESC |
| startDate | string |             |
| endDate   | string |             |

## Video Tutorials

Those video tutorials cover the main use cases for implementing the Flowise API.

{% embed url="https://youtu.be/9R5zo3IVkqU?si=y1v_aCQLE_70WBnA" %}

{% embed url="https://youtu.be/LhN560DhlzU" %}

{% embed url="https://youtu.be/kOwmPe8aLAA" %}
