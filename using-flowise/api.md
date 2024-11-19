---
description: >-
  Learn more about the details of some of the most used APIs: prediction,
  vector-upsert
---

# API

Refer to [API Reference](../api-reference/) for full list of public APIs

## Prediction

<div data-full-width="false">

<figure><img src="../.gitbook/assets/image (16) (1) (1).png" alt=""><figcaption></figcaption></figure>

</div>

{% swagger src="../.gitbook/assets/swagger (1) (1).yml" path="/prediction/{id}" method="post" %}
[swagger (1) (1).yml](<../.gitbook/assets/swagger (1) (1).yml>)
{% endswagger %}

### Using Python/TS Library

Flowise provides 2 libraries:

* [Python](https://pypi.org/project/flowise/): `pip install flowise`
* [Typescript](https://www.npmjs.com/package/flowise-sdk): `npm install flowise-sdk`

{% tabs %}
{% tab title="Python SDK" %}
```python
from flowise import Flowise, PredictionData

def test_non_streaming():
    client = Flowise()

    # Test non-streaming prediction
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="What is the capital of France?",
            streaming=False
        )
    )

    # Process and print the response
    for response in completion:
        print("Non-streaming response:", response)

def test_streaming():
    client = Flowise()

    # Test streaming prediction
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="Tell me a joke!",
            streaming=True
        )
    )

    # Process and print each streamed chunk
    print("Streaming response:")
    for chunk in completion:
        print(chunk)


if __name__ == "__main__":
    # Run non-streaming test
    test_non_streaming()

    # Run streaming test
    test_streaming()
```
{% endtab %}

{% tab title="Typescript SDK" %}
```javascript
import { FlowiseClient } from 'flowise-sdk'

async function test_streaming() {
  const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });

  try {
    // For streaming prediction
    const prediction = await client.createPrediction({
      chatflowId: 'fe1145fa-1b2b-45b7-b2ba-bcc5aaeb5ffd',
      question: 'What is the revenue of Apple?',
      streaming: true,
    });

    for await (const chunk of prediction) {
        console.log(chunk);
    }
    
  } catch (error) {
    console.error('Error:', error);
  }
}

async function test_non_streaming() {
    const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });
  
    try {
      // For streaming prediction
      const prediction = await client.createPrediction({
        chatflowId: 'fe1145fa-1b2b-45b7-b2ba-bcc5aaeb5ffd',
        question: 'What is the revenue of Apple?',
      });
  
      console.log(prediction);
      
    } catch (error) {
      console.error('Error:', error);
    }
}

// Run non-streaming test
test_non_streaming()

// Run streaming test
test_streaming()
```
{% endtab %}
{% endtabs %}

### Override Config

Override existing input configuration of the chatflow with **overrideConfig** property.

Due to security reason, override config is disabled by default. User has to enable this by going into Chatflow Configuration Security Tab. Then select the property that can be overriden.

<figure><img src="../.gitbook/assets/image (188).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

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

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

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

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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

You can pass a `sessionId` to persists the state of the conversation, so the every subsequent API calls will have context about previous conversation. Otherwise, a new session will be generated each time.

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

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

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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

Pass variables in the API to be used by the nodes in the flow. See more: [Variables](api.md#variables)

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

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

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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

<div align="left" data-full-width="false">

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="255"><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2024-02-29 011714.png" alt="" width="290"><figcaption></figcaption></figure>

</div>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

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

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2024-02-29 012538.png" alt="" width="431"><figcaption></figcaption></figure>

</div>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

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

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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

## Vector Upsert API

{% swagger src="../.gitbook/assets/swagger (1) (1).yml" path="/vector/upsert/{id}" method="post" %}
[swagger (1) (1).yml](<../.gitbook/assets/swagger (1) (1).yml>)
{% endswagger %}

### Document Loaders with File Upload

Some document loaders in Flowise allow user to upload files:

* [CSV File](../integrations/langchain/document-loaders/csv-file.md)
* [Docx File](../integrations/langchain/document-loaders/docx-file.md)
* [Json File](../integrations/langchain/document-loaders/json-file.md)
* [Json Lines File](../integrations/langchain/document-loaders/json-lines-file.md)
* [PDF File](../integrations/langchain/document-loaders/pdf-file.md)
* [Text File](../integrations/langchain/document-loaders/text-file.md)
* [Unstructured File](../integrations/langchain/document-loaders/unstructured-file-loader.md)

<div data-full-width="false">

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

</div>

If the flow contains [Document Loaders](../integrations/langchain/document-loaders/) with Upload File functionality, the API looks slightly different. Instead of passing body as JSON, **form data** is being used. This allows you to send files to the API.

{% hint style="info" %}
Make sure the sent file type is compatible with the expected file type from document loader. For example, if a PDF File Loader is being used, you should only send **.pdf** files.

To avoid having separate loaders for different file types, we recommend to use [File Loader](../integrations/langchain/document-loaders/file-loader.md)
{% endhint %}

{% tabs %}
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowId>"

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

{% tab title="Javascript API" %}
```javascript
// use FormData to upload files
let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("returnSourceDocuments", true);

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowId>",
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
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowId>"

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

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowId>",
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

## Video Tutorials

Those video tutorials cover the main use cases for implementing the Flowise API.

{% embed url="https://youtu.be/9R5zo3IVkqU?si=y1v_aCQLE_70WBnA" %}

{% embed url="https://youtu.be/LhN560DhlzU" %}

{% embed url="https://youtu.be/kOwmPe8aLAA" %}
