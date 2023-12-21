# API

### Prediction API

* POST `/api/v1/prediction/{your-chatflowid}`

<table><thead><tr><th width="187">Key</th><th width="338">Description</th><th>Type</th><th>Required</th></tr></thead><tbody><tr><td>question</td><td>User's question</td><td>string</td><td>Yes</td></tr><tr><td>overrideConfig</td><td>Override existing flow configuration</td><td>object</td><td>No</td></tr><tr><td>history</td><td>Provide list of history messages to the flow. Only works when using <a href="../integrations/memory/short-term-memory.md">Short Term Memory</a></td><td>array</td><td>No</td></tr></tbody></table>

You can use the chatflow as API and connect to frontend applications.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

You also have the flexibility to override input configuration with **overrideConfig** property.

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

```json
// Example body request
{
    "question": "What's my name?",
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
}
```

### Vector Upsert API

* POST `/api/v1/vector/upsert/{your-chatflowid}`

If the flow contains [Document Loaders](../integrations/document-loaders/), the API looks slightly different. Instead of passing body as **JSON**, **form-data** is being used. This allows you to upload any files to the API.

{% hint style="info" %}
It is user's responsibility to make sure the file type is compatible with the expected file type from document loader. For example, if a Text File Loader is being used, you should only upload file with `.txt` extension.
{% endhint %}

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

An example of API call with `form-data` using Postman:

<figure><img src="../.gitbook/assets/image (1) (4).png" alt=""><figcaption></figcaption></figure>

An example of API call using Python

```python
import requests

API_URL = "your-flowise-api"

# use form data to upload files
form_data = {
    "files": ('state_of_the_union.txt', open('state_of_the_union.txt', 'rb'))
}

body_data = {
    "question": "what is this document about?",
    "returnSourceDocuments": True
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```

### Message API

* GET `/api/v1/chatmessage/{your-chatflowid}`
* DELETE `/api/v1/chatmessage/{your-chatflowid}`

| Query Param | Type   | Value       |
| ----------- | ------ | ----------- |
| sessionId   | string |             |
| sort        | enum   | ASC or DESC |
| startDate   | string |             |
| endDate     | string |             |

### Tutorials

* How to use Flowise API

{% embed url="https://youtu.be/LhN560DhlzU" %}

* How to use Flowise API and connect to [Bubble](https://bubble.io/)

{% embed url="https://youtu.be/kOwmPe8aLAA" %}
