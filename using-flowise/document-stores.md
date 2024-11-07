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

<figure><img src="../.gitbook/assets/dastore011.png" alt=""><figcaption></figcaption></figure>

## 10. Summary

We started by creating a Document Store to organize the LibertyGuard Deluxe Homeowners Policy data. This data was then prepared by uploading, chunking, processing, and upserting it, making it ready for our RAG system.

**Advantages of the Document Store:**

Document Stores offer several benefits for managing and preparing data for Retrieval Augmented Generation (RAG) systems:

* **Organization and Management:** They provide a central location for storing, managing, and preparing your data.
* **Data Quality:** The chunking process helps structure data for accurate retrieval and analysis.
* **Flexibility:** Document Stores allow for refining and adjusting data as needed, improving the accuracy and relevance of your RAG system.

## 11. Video Tutorials

### RAG Like a Boss - Flowise Document Store Tutorial

In this video, [Leon](https://youtube.com/@leonvanzyl) provides a step by step tutorial on using Document Stores to easily manage your RAG knowledge bases in FlowiseAI.

{% embed url="https://youtu.be/PLuSfAkOHOA" %}
