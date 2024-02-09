# API

### Prediction API

* POST `/api/v1/prediction/{your-chatflowid}`

Request Body

<table><thead><tr><th width="192">Key</th><th width="559">Description</th><th>Type</th><th>Required</th></tr></thead><tbody><tr><td>question</td><td>User's question</td><td>string</td><td>Yes</td></tr><tr><td>overrideConfig</td><td>Override existing flow configuration</td><td>object</td><td>No</td></tr><tr><td>history</td><td>Provide list of history messages to the flow. Only works when using <a href="../integrations/langchain/memory/short-term-memory.md">Short Term Memory</a></td><td>array</td><td>No</td></tr></tbody></table>

You can use the chatflow as API and connect to frontend applications.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

You also have the flexibility to override input configuration with **overrideConfig** property.

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Python" %}
<pre class="language-python"><code class="lang-python"><strong>import requests
</strong>
API_URL = "http://localhost:3000/api/v1/prediction/&#x3C;chatlfowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "returnSourceDocuments": true
    },
    "history": [
        {
            "message": "Hello, how can I assist you?",
            "type": "apiMessage"
        },
        {
            "type": "userMessage",
            "message": "Hello I am Bob"
        },
        {
            "type": "apiMessage",
            "message": "Hello Bob! how can I assist you?"
        }
    ]
})
</code></pre>
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
        "returnSourceDocuments": true
    },
    "history": [
        {
            "message": "Hello, how can I assist you?",
            "type": "apiMessage"
        },
        {
            "type": "userMessage",
            "message": "Hello I am Bob"
        },
        {
            "type": "apiMessage",
            "message": "Hello Bob! how can I assist you?"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Vector Upsert API

* POST `/api/v1/vector/upsert/{your-chatflowid}`

Request Body

<table><thead><tr><th width="192">Key</th><th width="559">Description</th><th>Type</th><th>Required</th></tr></thead><tbody><tr><td>overrideConfig</td><td>Override existing flow configuration</td><td>object</td><td>No</td></tr><tr><td>stopNodeId</td><td>Node ID of the vector store. When you have multiple vector stores in a flow, you might not want to upsert all of them. Specifying <code>stopNodeId</code> will ensure only that specific vector store node is upserted.</td><td>array</td><td>No</td></tr></tbody></table>

#### Document Loaders with Upload

Some document loaders in Flowise allow user to upload files:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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

#### Document Loaders without Upload

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

### Message API

* GET `/api/v1/chatmessage/{your-chatflowid}`
* DELETE `/api/v1/chatmessage/{your-chatflowid}`

Query Parameters

| Param     | Type   | Value       |
| --------- | ------ | ----------- |
| sessionId | string |             |
| sort      | enum   | ASC or DESC |
| startDate | string |             |
| endDate   | string |             |

### Tutorials

* How to use Flowise API

{% embed url="https://youtu.be/LhN560DhlzU" %}

* How to use Flowise API and connect to [Bubble](https://bubble.io/)

{% embed url="https://youtu.be/kOwmPe8aLAA" %}
