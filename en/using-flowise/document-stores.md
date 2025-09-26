---
description: Learn how to use the Flowise Document Stores, written by @toi500
---

# Document Stores

***

Flowise's Document Stores offer a versatile approach to data management, enabling you to upload, split, and prepare your dataset and upsert it in a single location.

This centralized approach simplifies data handling and allows for efficient management of various data formats, making it easier to organize and access your data within the Flowise app.

## Setup

In this tutorial, we will set up a [Retrieval Augmented Generation (RAG)](broken-reference/) system to retrieve information about the _LibertyGuard Deluxe Homeowners Policy_, a topic that LLMs are not extensively trained on.

Using the **Flowise Document Stores**, we'll prepare and upsert data about LibertyGuard and its set of home insurance policies. This will enable our RAG system to accurately answer user queries about LibertyGuard's home insurance offerings.

## 1. Add a Document Store

Start by adding a Document Store and naming it. In our case, "LibertyGuard Deluxe Homeowners Policy".

<figure><img src="../.gitbook/assets/ds01.png" alt=""><figcaption></figcaption></figure>

## 2. Select a Document Loader

Enter the Document Store that you just created and select the [Document Loader](../integrations/langchain/document-loaders/) you want to use. In our case, since our dataset is in PDF format, we'll use the [PDF Loader](../integrations/langchain/document-loaders/pdf-file.md).

Document Loaders are specialized nodes that handle the ingestion of various document formats.

<figure><img src="../.gitbook/assets/ds02.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/ds03.png" alt=""><figcaption></figcaption></figure>

## 3. Prepare Your Data

### Step 1: Document Loader

* First, we start by uploading our PDF file.
* Then, we add a **unique metadata key**. This is optional, but a good practice as it allows us to target and filter down this same dataset later on if we need to.
* Every loader comes with preconfigured metadata, in some cases you can use Omit Metadata Keys to remove unnecessary metadata.

<figure><img src="../.gitbook/assets/ds04.png" alt=""><figcaption></figcaption></figure>

### Step 2: Text Splitter

* Select the [Text Splitter](../integrations/langchain/text-splitters/) you want to use to chunk your data. In our particular case, we will use the [Recursive Character Text Splitter](../integrations/langchain/text-splitters/recursive-character-text-splitter.md).
*   Text splitter is used to split the loaded documents into smaller pieces, documents, or chunks. This is a crucial preprocessing step for 2 main reasons:

    * **Retrieval speed and relevance:** Storing and querying large documents as single entities in a vector database can lead to slower retrieval times and potentially less relevant results. Splitting the document into smaller chunks allows for more targeted retrieval. By querying against smaller, more focused units of information, we can achieve faster response times and improve the precision of the retrieved results.
    * **Cost-effective:** Since we only retrieve relevant chunks rather than the entire document, the number of tokens processed by the LLM is significantly reduced. This targeted retrieval approach directly translates to lower usage costs for our LLM, as billing is typically based on token consumption. By minimizing the amount of irrelevant information sent to the LLM, we also optimize for cost.

    There are different text chunking strategies, including:

    * **Character Text Splitting:** Dividing the text into chunks of a fixed number of characters. This method is straightforward but may split words or phrases across chunks, potentially disrupting context.
    * **Token Text Splitting:** Segmenting the text based on word boundaries or tokenization schemes specific to the chosen embedding model. This approach often leads to more semantically coherent chunks, as it preserves word boundaries and considers the underlying linguistic structure of the text.
    * **Recursive Character Text Splitting:** This strategy aims to divide text into chunks that maintain semantic coherence while staying within a specified size limit. It's particularly well-suited for hierarchical documents with nested sections or headings. Instead of blindly splitting at the character limit, it recursively analyzes the text to find logical breakpoints, such as sentence endings or section breaks. This approach ensures that each chunk represents a meaningful unit of information, even if it slightly exceeds the target size.
    * **Markdown Text Splitter:** Designed specifically for markdown-formatted documents, this splitter logically segments the text based on markdown headings and structural elements, creating chunks that correspond to logical sections within the document.
    * **Code Text Splitter:** Tailored for splitting code files, this strategy considers code structure, function definitions, and other programming language-specific elements to create meaningful chunks that are suitable for tasks like code search and documentation.
    * **HTML-to-Markdown Text Splitter:** This specialized splitter first converts HTML content to Markdown and then applies the Markdown Text Splitter, allowing for structured segmentation of web pages and other HTML documents.

    You can also customize the parameters such as:

    * **Chunk Size:** The desired maximum size of each chunk, usually defined in characters or tokens.
    * **Chunk Overlap:** The number of characters or tokens to overlap between consecutive chunks, useful for maintaining contextual flow across chunks.

{% hint style="info" %}
In this guide, we've added a generous **Chunk Overlap** size to ensure no relevant data gets missed between chunks. However, the optimal overlap size is dependent on the complexity of your data. You may need to adjust this value based on your specific dataset and the nature of the information you want to extract.
{% endhint %}

<figure><img src="../.gitbook/assets/ds05.png" alt="" width="563"><figcaption></figcaption></figure>

## 4. Preview Your Data

We can now preview how our data will be chunked using our current [Text Splitter](../integrations/langchain/text-splitters/) configuration; `chunk_size=1500`and `chunk_overlap=750`.

<figure><img src="../.gitbook/assets/ds06.png" alt=""><figcaption></figcaption></figure>

It's important to experiment with different [Text Splitters](../integrations/langchain/text-splitters/), Chunk Sizes, and Overlap values to find the optimal configuration for your specific dataset. This preview allows you to refine the chunking process and ensure that the resulting chunks are suitable for your RAG system.

<figure><img src="../.gitbook/assets/ds07.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note that our custom metadata `company: "liberty"` has been inserted into each chunk. This metadata allows us to easily filter and retrieve information from this specific dataset later on, even if we use the same vector store index for other datasets.
{% endhint %}

### Understanding Chunk Overlap <a href="#understanding-chunk-overlap" id="understanding-chunk-overlap"></a>

In the context of vector-based retrieval and LLM querying, chunk overlap plays an **important role in maintaining contextual continuity** and **improving response accuracy**, especially when dealing with limited retrieval depth or **top K**, which is the parameter that determines the maximum number of most similar chunks that are retrieved from the [Vector Store](https://docs.flowiseai.com/integrations/langchain/vector-stores) in response to a query.

During query processing, the LLM executes a similarity search against the Vector Store to retrieve the most semantically relevant chunks to the given query. If the retrieval depth, represented by the top K parameter, is set to a small value, 4 for default, the LLM initially uses information only from these 4 chunks to generate its response.

This scenario presents us with a problem, since relying solely on a limited number of chunks without overlap can lead to incomplete or inaccurate answers, particularly when dealing with queries that require information spanning multiple chunks.

Chunk overlap helps with this issue by ensuring that a portion of the textual context is shared across consecutive chunks, **increasing the likelihood that all relevant information for a given query is contained within the retrieved chunks**.

In other words, this overlap serves as a bridge between chunks, enabling the LLM to access a wider contextual window even when limited to a small set of retrieved chunks (top K). If a query relates to a concept or piece of information that extends beyond a single chunk, the overlapping regions increase the likelihood of capturing all the necessary context.

Therefore, by introducing chunk overlap during the text splitting phase, we enhance the LLM's ability to:

1. **Preserve contextual continuity:** Overlapping chunks provide a smoother transition of information between consecutive segments, allowing the model to maintain a more coherent understanding of the text.
2. **Improve retrieval accuracy:** By increasing the probability of capturing all relevant information within the target top K retrieved chunks, overlap contributes to more accurate and contextually appropriate responses.

### Accuracy vs. Cost <a href="#accuracy-vs.-cost" id="accuracy-vs.-cost"></a>

So, to further optimize the trade-off between retrieval accuracy and cost, two primary strategies can be used:

1. **Increase/Decrease Chunk Overlap:** Adjusting the overlap percentage during text splitting allows for fine-grained control over the amount of shared context between chunks. Higher overlap percentages generally lead to improved context preservation but may also increase costs since you would need to use more chunks to encompass the entire document. Conversely, lower overlap percentages can reduce costs but risk losing key contextual information between chunks, potentially leading to less accurate or incomplete answers from the LLM.
2. **Increase/Decrease Top K:** Raising the default top K value (4) expands the number of chunks considered for response generation. While this can improve accuracy, it also increases cost.

**Tip:** The choice of optimal **overlap** and **top K** values depends on factors such as document complexity, embedding model characteristics, and the desired balance between accuracy and cost. Experimentation with these values is important for finding the ideal configuration for a specific need.

## 5. Process Your Data

Once you are satisfied with the chunking process, it's time to process your data.

<figure><img src="../.gitbook/assets/ds08.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/ds09%20(1).png" alt=""><figcaption></figcaption></figure>

After processing your data, you retain the ability to refine individual chunks by deleting or adding content. This granular control offers several advantages:

* **Enhanced Accuracy:** Identify and rectify inaccuracies or inconsistencies present in the original data, ensuring the information used in your application is reliable.
* **Improved Relevance:** Refine chunk content to emphasize key information and remove irrelevant sections, thereby increasing the precision and effectiveness of your retrieval process.
* **Query Optimization:** Tailor chunks to better align with anticipated user queries, making them more targeted and improving the overall user experience.

## 6. Configure the Upsert Process

With our data properly processed - loaded via a Document Loader and appropriately chunked -, we can now proceed to configure the upsert process.

<figure><img src="../.gitbook/assets/dastore002.png" alt=""><figcaption></figcaption></figure>

The upsert process comprises three fundamental steps:

* **Embedding:** We begin by choosing the appropriate embedding model to encode our dataset. This model will transform our data into a numerical vector representation.
* **Vector Store:** Next, we determine the Vector Store where our dataset will reside.
* **Record Manager (Optional):** Finally, we have the option to implement a Record Manager. This component provides the functionalities for managing our dataset once it's stored within the Vector Store.

<figure><img src="../.gitbook/assets/dastore003.png" alt=""><figcaption></figcaption></figure>

### Step 1: Select Embeddings

Click on the "Select Embeddings" card and choose your preferred [embedding model](../integrations/langchain/embeddings/). In our case, we will select OpenAI as the embedding provider and use the `text-embedding-ada-002` model with `1536` dimensions.

Embedding is the process of converting text into a numerical representation that captures its meaning. This numerical representation, also called the embedding vector, is a multi-dimensional array of numbers, where each dimension represents a specific aspect of the text's meaning.

These vectors allow LLMs to compare and search for similar pieces of text within the vector store by measuring the distance or similarity between them in this multi-dimensional space.

#### Understanding Embeddings/Vector Store dimensions <a href="#understanding-embeddings-vector-store-dimensions" id="understanding-embeddings-vector-store-dimensions"></a>

The number of dimensions in a Vector Store index is determined by the embedding model used when we upsert our data, and vice versa. Each dimension represents a specific feature or concept within the data. For example, a **dimension** might **represent a particular topic, sentiment, or other aspect of the text**.

The more dimensions we use to embed our data, the greater the potential for capturing nuanced meaning from our text. However, this increase comes at the cost of higher computational requirements per query.

In general, a larger number of dimensions needs more resources to store, process, and compare the resulting embedding vectors. Therefore, embeddings models like the Google `embedding-001`, which uses 768 dimensions, are, in theory, cheaper than others like the OpenAI `text-embedding-3-large`, with 3072 dimensions.

It's important to note that the **relationship between dimensions and meaning capture isn't strictly linear**; there's a point of diminishing returns where adding more dimensions provides negligible benefit for the added unnecessary cost.

{% hint style="warning" %}
To ensure compatibility between an embedding model and a Vector Store index, dimensional alignment is essential. Both **the embedding model and the vector store index must have the same number of dimensions**. Dimensionality mismatch will result in upsertion errors, as the Vector Store is designed to handle vectors of a specific size determined by the chosen embedding model.
{% endhint %}

<figure><img src="../.gitbook/assets/dastore004.png" alt=""><figcaption></figcaption></figure>

### Step 2: Select Vector Store

Click on the "Select Vector Store" card and choose your preferred [Vector Store](../integrations/langchain/vector-stores/). In our case, as we need a production-ready option, we will select Upstash.

Vector store is a special type of database that is used to store the vector embeddings. We can finetune parameters like "**top K**" that determines the maximum number of most similar chunks that are retrieved from the Vector Store in response to a query.

{% hint style="info" %}
A lower top K value will yield fewer but potentially more relevant results, while a higher value will return a broader range of results, potentially capturing more information.
{% endhint %}

<figure><img src="../.gitbook/assets/dastore005.png" alt=""><figcaption></figcaption></figure>

### Step 3: Select Record Manager

Record Manager is an optional but incredibly useful addition to our upserting flow. It allows us to maintain records of all the chunks that have been upserted to our Vector Store, enabling us to efficiently add or delete chunks as needed.

In other words, any changes to your documents during a new upsert will not result in duplicate vector embeddings being stored in the vector store.

Detailed instructions on how to set up and utilize this feature can be found in the dedicated [guide](../integrations/langchain/record-managers.md).

<figure><img src="../.gitbook/assets/dastore006.png" alt=""><figcaption></figcaption></figure>

## 7. Upsert Your Data to a Vector Store

To begin the upsert process and transfer your data to the Vector Store, click the "Upsert" button.

<figure><img src="../.gitbook/assets/dastore013.png" alt=""><figcaption></figcaption></figure>

As illustrated in the image below, our data has been successfully upserted into the Upstash vector database. The data was divided into 85 chunks to optimize the upsertion process and ensure efficient storage and retrieval.

<figure><img src="../.gitbook/assets/dastore007.png" alt="" width="375"><figcaption></figcaption></figure>

## 8. Test Your Dataset

To quickly test the functionality of your dataset without navigating away from the Document Store, simply utilize the "Retrieval Query" button. This initiates a test query, allowing you to verify the accuracy and effectiveness of your data retrieval process.

<figure><img src="../.gitbook/assets/dastore010.png" alt=""><figcaption></figcaption></figure>

In our case, we see that when querying for information about kitchen flooring coverage in our insurance policy, we retrieve 4 relevant chunks from Upstash, our designated Vector Store. This retrieval is limited to 4 chunks as per the defined "top k" parameter, ensuring we receive the most pertinent information without unnecessary redundancy.

<figure><img src="../.gitbook/assets/dastore009.png" alt=""><figcaption></figcaption></figure>

## 9. Test Your RAG

Finally, our Retrieval-Augmented Generation (RAG) system is operational. It's noteworthy how the LLM effectively interprets the query and successfully leverages relevant information from the chunked data to construct a comprehensive response.

#### Agentflow

With an Agent node, you can add the document store:

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1).png" alt="" width="300"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt="" width="407"><figcaption></figcaption></figure>

Or directly connect to vector database and embedding mode:

<figure><img src="../.gitbook/assets/image (6) (1) (1).png" alt="" width="394"><figcaption></figcaption></figure>

#### Chatflow

You can use the vector store that was configured earlier:

<figure><img src="../.gitbook/assets/dastore011.png" alt=""><figcaption></figcaption></figure>

Or, use the Document Store (Vector):

<figure><img src="../.gitbook/assets/image (215).png" alt=""><figcaption></figcaption></figure>

## 10. API

There are also APIs support for creating, updating and deleting document store. In this section, we are going to highlight the 2 of the most used APIs:

* Upsert
* Refresh

For details, see the [Document Store API Reference](../api-reference/document-store.md).

### Upsert API

There are a few different scenarios for upserting process, and each have different outcomes.

#### Scenario 1: In the same document store, use an existing document loader configuration, upsert as new document loader.

<figure><img src="../.gitbook/assets/Untitled-2025-02-02-1727.png" alt="" width="496"><figcaption></figcaption></figure>

{% hint style="success" %}
**`docId`** represents the existing document loader ID. It is required in the request body for this scenario.
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
**`docId`** and **`replaceExisting`** are both required in the request body for this scenario.
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

<figure><img src="../.gitbook/assets/Picture1 (1).png" alt=""><figcaption></figcaption></figure>

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
Make sure the sent file type is compatible with the expected file type from document loader.

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
