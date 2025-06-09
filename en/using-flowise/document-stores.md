---
description: Learn how to use the Flowise Document Stores, written by @toi500
---

# Document Stores

***

Flowise's Document Stores offer a versatile approach to data management, enabling you to upload, split, and prepare your dataset and upsert it in a single location.

This centralized approach simplifies data handling and allows for efficient management of various data formats, making it easier to organize and access your data within the Flowise app.

## Setup

In this tutorial, we will set up a [Retrieval Augmented Generation (RAG)](../use-cases/multiple-documents-qna.md) system to retrieve information about the _LibertyGuard Deluxe Homeowners Policy_, a topic that LLMs are not extensively trained on.

Using the **Flowise Document Stores**, we'll prepare and upsert data about LibertyGuard and its set of home insurance policies. This will enable our RAG system to accurately answer user queries about LibertyGuard's home insurance offerings.

## 1. Add a Document Store

* Start by adding a Document Store and naming it. In our case, "LibertyGuard Deluxe Homeowners Policy".

<figure><img src="../.gitbook/assets/ds01.png" alt=""><figcaption></figcaption></figure>

## 2. Select a Document Loader

* Enter the Document Store that you just created and select the [Document Loader](../integrations/langchain/document-loaders/) you want to use. In our case, since our dataset is in PDF format, we'll use the [PDF Loader](../integrations/langchain/document-loaders/pdf-file.md).

<figure><img src="../.gitbook/assets/ds02.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/ds03.png" alt=""><figcaption></figcaption></figure>

## 3. Prepare Your Data

* First, we start by uploading our PDF file.
* Then, we add a **unique metadata key**. This is optional, but a good practice as it allows us to target and filter down this same dataset later on if we need to.

<figure><img src="../.gitbook/assets/ds04.png" alt=""><figcaption></figcaption></figure>

* Finally, select the [Text Splitter](../integrations/langchain/text-splitters/) you want to use to chunk your data. In our particular case, we will use the [Recursive Character Text Splitter](../integrations/langchain/text-splitters/recursive-character-text-splitter.md).

{% hint style="info" %}
In this guide, we've added a generous **Chunk Overlap** size to ensure no relevant data gets missed between chunks. However, the optimal overlap size is dependent on the complexity of your data. You may need to adjust this value based on your specific dataset and the nature of the information you want to extract. More about this topic in this [guide](../use-cases/upserting-data.md).
{% endhint %}

<figure><img src="../.gitbook/assets/ds05.png" alt=""><figcaption></figcaption></figure>

## 4. Preview Your Data

* We can now preview how our data will be chunked using our current [Text Splitter](../integrations/langchain/text-splitters/) configuration; `chunk_size=1500`and `chunk_overlap=750`.

<figure><img src="../.gitbook/assets/ds06.png" alt=""><figcaption></figcaption></figure>

* It's important to experiment with different [Text Splitters](../integrations/langchain/text-splitters/), Chunk Sizes, and Overlap values to find the optimal configuration for your specific dataset. This preview allows you to refine the chunking process and ensure that the resulting chunks are suitable for your RAG system.

<figure><img src="../.gitbook/assets/ds07.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note that our custom metadata `company: "liberty"` has been inserted into each chunk. This metadata allows us to easily filter and retrieve information from this specific dataset later on, even if we use the same vector store index for other datasets.
{% endhint %}

## 5. Process Your Data

* Once you are satisfied with the chunking process, it's time to process your data.

<figure><img src="../.gitbook/assets/ds08.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/ds09%20(1).png" alt=""><figcaption></figcaption></figure>

After processing your data, you retain the ability to refine individual chunks by deleting or adding content. This granular control offers several advantages:

* **Enhanced Accuracy:** Identify and rectify inaccuracies or inconsistencies present in the original data, ensuring the information used in your application is reliable.
* **Improved Relevance:** Refine chunk content to emphasize key information and remove irrelevant sections, thereby increasing the precision and effectiveness of your retrieval process.
* **Query Optimization:** Tailor chunks to better align with anticipated user queries, making them more targeted and improving the overall user experience.

## 6. Configure the Upsert Process

* With our data properly processed - loaded via a Document Loader and appropriately chunked -, we can now proceed to configure the upsert process.

<figure><img src="../.gitbook/assets/dastore002.png" alt=""><figcaption></figcaption></figure>

The upsert process comprises three fundamental steps:

* **Embedding Selection:** We begin by choosing the appropriate embedding model to encode our dataset. This model will transform our data into a numerical vector representation.
* **Data Store Selection:** Next, we determine the Vector Store where our dataset will reside.
* **Record Manager Selection (Optional):** Finally, we have the option to implement a Record Manager. This component provides the functionalities for managing our dataset once it's stored within the Vector Store.

<figure><img src="../.gitbook/assets/dastore003.png" alt=""><figcaption></figcaption></figure>

### 1. Select Embeddings

* Click on the "Select Embeddings" card and choose your preferred [embedding model](../integrations/langchain/embeddings/). In our case, we will select OpenAI as the embedding provider and use the "text-embedding-ada-002" model with 1536 dimensions.

<figure><img src="../.gitbook/assets/dastore004.png" alt=""><figcaption></figcaption></figure>

### 2. Select Vector Store

* Click on the "Select Vector Store" card and choose your preferred [Vector Store](../integrations/langchain/vector-stores/). In our case, as we need a production-ready option, we will select Upstash.

<figure><img src="../.gitbook/assets/dastore005.png" alt=""><figcaption></figcaption></figure>

### 3. Select Record Manager

* For advanced dataset management within the Vector Store, you can optionally select and configure a [Record Manager](../integrations/langchain/record-managers.md). Detailed instructions on how to set up and utilize this feature can be found in the dedicated [guide](../integrations/langchain/record-managers.md).

<figure><img src="../.gitbook/assets/dastore006.png" alt=""><figcaption></figcaption></figure>

## 7. Upsert Your Data to a Vector Store

* To begin the upsert process and transfer your data to the Vector Store, click the "Upsert" button.

<figure><img src="../.gitbook/assets/dastore013.png" alt=""><figcaption></figcaption></figure>

* As illustrated in the image below, our data has been successfully upserted into the Upstash vector database. The data was divided into 85 chunks to optimize the upsertion process and ensure efficient storage and retrieval.

<figure><img src="../.gitbook/assets/dastore007.png" alt="" width="375"><figcaption></figcaption></figure>

## 8. Test Your Dataset

* To quickly test the functionality of your dataset without navigating away from the Document Store, simply utilize the "Retrieval Query" button. This initiates a test query, allowing you to verify the accuracy and effectiveness of your data retrieval process.

<figure><img src="../.gitbook/assets/dastore010.png" alt=""><figcaption></figcaption></figure>

* In our case, we see that when querying for information about kitchen flooring coverage in our insurance policy, we retrieve 4 relevant chunks from Upstash, our designated Vector Store. This retrieval is limited to 4 chunks as per the defined "top k" parameter, ensuring we receive the most pertinent information without unnecessary redundancy.

<figure><img src="../.gitbook/assets/dastore009.png" alt=""><figcaption></figcaption></figure>

## 9. Test Your RAG

* Finally, our Retrieval-Augmented Generation (RAG) system is operational. It's noteworthy how the LLM effectively interprets the query and successfully leverages relevant information from the chunked data to construct a comprehensive response.

You can use the vector store that was configured earlier:

<figure><img src="../.gitbook/assets/dastore011.png" alt=""><figcaption></figcaption></figure>

Or, use the Document Store (Vector):

<figure><img src="../.gitbook/assets/image (215).png" alt=""><figcaption></figcaption></figure>

## 10. API

There are also APIs support for creating, updating and deleting document store. Refer to [Document Store API](../api-reference/document-store.md) for more details. In this section, we are going to highlight the 2 of the most used APIs: upsert and refresh.

### Upsert API

There are a few different scenarios for upserting process, and each have different outcomes.

#### Scenario 1: In the same document store, use an existing document loader configuration, upsert as new document loader.

<figure><img src="../.gitbook/assets/Untitled-2025-02-02-1727.png" alt="" width="496"><figcaption></figcaption></figure>

{% hint style="success" %}
**`docId`** represents the existing document loader ID. It is required in the request body for this scenario.&#x20;
{% endhint %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

DOC_STORE_ID = "your_doc_store_id"
DOC_LOADER_ID = "your_doc_loader_id"
API_URL = f"http://localhost:3000/api/v1/document-store/upsert/{DOC_STORE_ID}"
API_KEY = "your_api_key_here"

form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

body_data = {
    "docId": DOC_LOADER_ID
}

headers = {
    "Authorization": f"Bearer {BEARER_TOKEN}"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data, headers=headers)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const DOC_STORE_ID = "your_doc_store_id"
const DOC_LOADER_ID = "your_doc_loader_id"

let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("docId", DOC_LOADER_ID)

async function query(formData) {
    const response = await fetch(
        `http://localhost:3000/api/v1/document-store/upsert/${DOC_STORE_ID}`,
        {
            method: "POST",
            headers: {
                "Authorization": "Bearer <your_api_key_here>"
            },
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

#### Scenario 2: In the same document store, replace an existing document loader with new files.

<figure><img src="../.gitbook/assets/Untitled-2025-03-02-1727.png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="success" %}
**`docId`** and **`replaceExisting`** are both required in the request body for this scenario.&#x20;
{% endhint %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

DOC_STORE_ID = "your_doc_store_id"
DOC_LOADER_ID = "your_doc_loader_id"
API_URL = f"http://localhost:3000/api/v1/document-store/upsert/{DOC_STORE_ID}"
API_KEY = "your_api_key_here"

form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

body_data = {
    "docId": DOC_LOADER_ID,
    "replaceExisting": True
}

headers = {
    "Authorization": f"Bearer {BEARER_TOKEN}"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data, headers=headers)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const DOC_STORE_ID = "your_doc_store_id";
const DOC_LOADER_ID = "your_doc_loader_id";

let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("docId", DOC_LOADER_ID);
formData.append("replaceExisting", true);

async function query(formData) {
    const response = await fetch(
        `http://localhost:3000/api/v1/document-store/upsert/${DOC_STORE_ID}`,
        {
            method: "POST",
            headers: {
                "Authorization": "Bearer <your_api_key_here>"
            },
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

#### Scenario 3: In the same document store, upsert as new document loader from scratch.

<figure><img src="../.gitbook/assets/Untitled-2025-04-02-1727.png" alt="" width="439"><figcaption></figcaption></figure>

{% hint style="success" %}
**`loader`, `splitter`, `embedding`, `vectorStore`** are all required in the request body for this scenario. **`recordManager`** is optional.
{% endhint %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

DOC_STORE_ID = "your_doc_store_id"
API_URL = f"http://localhost:3000/api/v1/document-store/upsert/{DOC_STORE_ID}"
API_KEY = "your_api_key_here"

form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

loader = {
    "name": "pdfFile",
    "config": {} # you can leave empty to use default config
}

splitter = {
    "name": "recursiveCharacterTextSplitter",
    "config": {
        "chunkSize": 1400,
        "chunkOverlap": 100
    }
}

embedding = {
    "name": "openAIEmbeddings",
    "config": {
        "modelName": "text-embedding-ada-002",
        "credential": <your_credential_id>
    }
}

vectorStore = {
    "name": "pinecone",
    "config": {
        "pineconeIndex": "exampleindex",
        "pineconeNamespace": "examplenamespace",
        "credential":  <your_credential_i
    }
}

body_data = {
    "docId": DOC_LOADER_ID,
    "loader": json.dumps(loader),
    "splitter": json.dumps(splitter),
    "embedding": json.dumps(embedding),
    "vectorStore": json.dumps(vectorStore)
}

headers = {
    "Authorization": f"Bearer {BEARER_TOKEN}"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data, headers=headers)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const DOC_STORE_ID = "your_doc_store_id";
const API_URL = `http://localhost:3000/api/v1/document-store/upsert/${DOC_STORE_ID}`;
const API_KEY = "your_api_key_here";

const formData = new FormData();
formData.append("files", new Blob([await (await fetch('my-another-file.pdf')).blob()]), "my-another-file.pdf");

const loader = {
    name: "pdfFile",
    config: {} // You can leave empty to use the default config
};

const splitter = {
    name: "recursiveCharacterTextSplitter",
    config: {
        chunkSize: 1400,
        chunkOverlap: 100
    }
};

const embedding = {
    name: "openAIEmbeddings",
    config: {
        modelName: "text-embedding-ada-002",
        credential: "your_credential_id"
    }
};

const vectorStore = {
    name: "pinecone",
    config: {
        pineconeIndex: "exampleindex",
        pineconeNamespace: "examplenamespace",
        credential: "your_credential_id"
    }
};

const bodyData = {
    docId: "DOC_LOADER_ID",
    loader: JSON.stringify(loader),
    splitter: JSON.stringify(splitter),
    embedding: JSON.stringify(embedding),
    vectorStore: JSON.stringify(vectorStore)
};

const headers = {
    "Authorization": `Bearer BEARER_TOKEN`
};

async function query() {
    try {
        const response = await fetch(API_URL, {
            method: "POST",
            headers: headers,
            body: formData
        });

        const result = await response.json();
        console.log(result);
        return result;
    } catch (error) {
        console.error("Error:", error);
    }
}

query();

```
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
Creating from scratch is not recommended as it exposes your credential ID. The recommended way is to create a placeholder document store and configure the parameters on the UI. Then use the placeholder as the base for adding new document loader or creating new document store.
{% endhint %}

#### Scenario 4: Create new document store for every upsert

<figure><img src="../.gitbook/assets/Untitled-2025-056-02-1727.png" alt="" width="533"><figcaption></figcaption></figure>

{% hint style="success" %}
**`createNewDocStore`** and **`docStore`** are both required in the request body for this scenario.
{% endhint %}

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

DOC_STORE_ID = "your_doc_store_id"
DOC_LOADER_ID = "your_doc_loader_id"
API_URL = f"http://localhost:3000/api/v1/document-store/upsert/{DOC_STORE_ID}"
API_KEY = "your_api_key_here"

form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

body_data = {
    "docId": DOC_LOADER_ID,
    "createNewDocStore": True,
    "docStore": json.dumps({"name":"My NEW Doc Store"})
}

headers = {
    "Authorization": f"Bearer {BEARER_TOKEN}"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data, headers=headers)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const DOC_STORE_ID = "your_doc_store_id";
const DOC_LOADER_ID = "your_doc_loader_id";

let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("docId", DOC_LOADER_ID);
formData.append("createNewDocStore", true);
formData.append("docStore", JSON.stringify({ "name": "My NEW Doc Store" }));

async function query(formData) {
    const response = await fetch(
        `http://localhost:3000/api/v1/document-store/upsert/${DOC_STORE_ID}`,
        {
            method: "POST",
            headers: {
                "Authorization": "Bearer <your_api_key_here>"
            },
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

#### Q: Where to find Document Store ID and Document Loader ID?

A: You can find the respective IDs from the URL.

<figure><img src="../.gitbook/assets/Picture1.png" alt=""><figcaption></figcaption></figure>

#### Q: Where can I find the available configs to override?

A: You can find the available configs from the **View API** button on each document loader:

<figure><img src="../.gitbook/assets/image (4) (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (6).png" alt=""><figcaption></figcaption></figure>

For each upsert, there are 5 elements involved:

* **`loader`**
* **`splitter`**
* **`embedding`**
* **`vectorStore`**
* **`recordManager`**

You can override existing configuration with the **`config`** body of the element. For example, using the screenshot above, you can create a new document loader with a new **`url`**:

{% tabs %}
{% tab title="Python" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/document-store/upsert/<storeId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "docId": <docLoaderId>,
    # override existing configuration
    "loader": {
        "config": {
            "url": "https://new-url.com"
        }
    }
})
print(output)
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/document-store/upsert/<storeId>",
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
    "docId": <docLoaderId>,
    // override existing configuration
    "loader": {
        "config": {
            "url": "https://new-url.com"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

What if the loader has file upload? Yes, you guessed it right, we have to use form data as body!

Using the image below as an example, we can override the **`usage`** parameter of the PDF File Loader like so:

<figure><img src="../.gitbook/assets/image (4) (3) (1).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

API_URL = "http://localhost:3000/api/v1/document-store/upsert/<storeId>"
API_KEY = "your_api_key_here"

form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

override_loader_config = {
    "config": {
        "usage": "perPage"
    }
}

body_data = {
    "docId": <docLoaderId>,
    "loader": json.dumps(override_loader_config) # Override existing configuration
}

headers = {
    "Authorization": f"Bearer {BEARER_TOKEN}"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data, headers=headers)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const DOC_STORE_ID = "your_doc_store_id";
const DOC_LOADER_ID = "your_doc_loader_id";

const overrideLoaderConfig = {
    "config": {
        "usage": "perPage"
    }
}

let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("docId", DOC_LOADER_ID);
formData.append("loader", JSON.stringify(overrideLoaderConfig));

async function query(formData) {
    const response = await fetch(
        `http://localhost:3000/api/v1/document-store/upsert/${DOC_STORE_ID}`,
        {
            method: "POST",
            headers: {
                "Authorization": "Bearer <your_api_key_here>"
            },
            body: formData
        }
    )
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});e
```
{% endtab %}
{% endtabs %}

#### Q: When to use Form Data vs JSON as the body of API request?

A: For [Document Loaders](../integrations/langchain/document-loaders/) that have File Upload functionality, such as PDF, DOCX, TXT, etc, body must be sent as Form Data.

{% hint style="warning" %}
Make sure the sent file type is compatible with the expected file type from document loader.&#x20;

For example, if a [PDF File Loader](../integrations/langchain/document-loaders/pdf-file.md) is being used, you should only send **.pdf** files.

To avoid having separate loaders for different file types, we recommend to use [File Loader](../integrations/langchain/document-loaders/file-loader.md)
{% endhint %}

{% tabs %}
{% tab title="Python API" %}
```python
import requests
import json

API_URL = "http://localhost:3000/api/v1/document-store/upsert/<storeId>"

# use form data to upload files
form_data = {
    "files": ('my-another-file.pdf', open('my-another-file.pdf', 'rb'))
}

body_data = {
    "docId": <docId>
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
formData.append("docId", <docId>);

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/document-store/upsert/<storeId>",
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

For other [Document Loaders](https://docs.flowiseai.com/integrations/langchain/document-loaders) nodes without Upload File functionality, the API body is in **JSON** format:

{% tabs %}
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/document-store/upsert/<storeId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query({
    "docId": <docId>
})
print(output)
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/document-store/upsert/<storeId>",
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
    "docId": <docId>
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

#### Q: Can I add new metadata?

A: You can provide new metadata by passing the **`metadata`** inside the body request:

```json
{
    "docId": <doc-id>,
    "metadata": {
        "source: "abc"
    }
}
```

### Refresh API

Often times you might want to re-process every documents loaders within document store to fetch the latest data, and upsert to vector store, to keep everything in sync. This can be done via Refresh API:

{% tabs %}
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/document-store/refresh/<storeId>"

def query():
    response = requests.post(API_URL)
    return response.json()

output = query()
print(output)
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/document-store/refresh/<storeId>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            }
        }
    );
    const result = await response.json();
    return result;
}

query().then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

You can also override existing configuration of specific document loader:

{% tabs %}
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/document-store/refresh/<storeId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()

output = query(
{
    "items": [
        {
            "docId": <docId>,
            "splitter": {
                "name": "recursiveCharacterTextSplitter",
                "config": {
                    "chunkSize": 2000,
                    "chunkOverlap": 100
                }
            }
        }
    ]
}
)
print(output)
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/document-store/refresh/<storeId>",
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
    "items": [
        {
            "docId": <docId>,
            "splitter": {
                "name": "recursiveCharacterTextSplitter",
                "config": {
                    "chunkSize": 2000,
                    "chunkOverlap": 100
                }
            }
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## 11. Summary

We started by creating a Document Store to organize the LibertyGuard Deluxe Homeowners Policy data. This data was then prepared by uploading, chunking, processing, and upserting it, making it ready for our RAG system.

**Advantages of the Document Store:**

Document Stores offer several benefits for managing and preparing data for Retrieval Augmented Generation (RAG) systems:

* **Organization and Management:** They provide a central location for storing, managing, and preparing your data.
* **Data Quality:** The chunking process helps structure data for accurate retrieval and analysis.
* **Flexibility:** Document Stores allow for refining and adjusting data as needed, improving the accuracy and relevance of your RAG system.

## 12. Video Tutorials

### RAG Like a Boss - Flowise Document Store Tutorial

In this video, [Leon](https://youtube.com/@leonvanzyl) provides a step by step tutorial on using Document Stores to easily manage your RAG knowledge bases in FlowiseAI.

{% embed url="https://youtu.be/PLuSfAkOHOA" %}
