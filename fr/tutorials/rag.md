# RAG

Large Language Models (LLMs) have unlocked the potential to create advanced Q\&A chatbots capable of delivering precise answers based on specific content. These systems rely on a method called Retrieval-Augmented Generation (RAG), which enhances their responses by grounding them in relevant source material.

In this tutorial, you’ll learn how to create a basic Q\&A application that can extract and answer questions from given document sources.

The process can be separated out into 2 sub-processes:

* Indexing
* Retrieval

## Indexing

[Document Stores](../using-flowise/document-stores.md) is designed to help with the whole indexing pipelines - retrieveing data from different sources, chunking strategy, upserting to vector database, syncing with updated data.

We support wide range of document loaders, ranging from files like Pdf, Word, Google Drive, to web scrapers like Playwright, Firecrawl, Apify and others. You can also create custom document loader.

<figure><img src="../.gitbook/assets/image (293).png" alt="" width="563"><figcaption></figcaption></figure>

## Retrieval

Based on the user's input, relevant document chunks are fetched from vector database. LLM then uses the retrieved context to generate a response.

1. Drag and drop an [Agent](../using-flowise/agentflowv2.md#id-3.-agent-node) node, and configure the model to use.

<figure><img src="../.gitbook/assets/image (290).png" alt="" width="391"><figcaption></figcaption></figure>

2. Add a new Knowledge (Document Store) and define what the content is about. This helps the LLM understand when and how to retrieve relevant information. You can also use the auto-generate button to assist with this process.

{% hint style="success" %}
Only upserted document store can be used
{% endhint %}

<figure><img src="../.gitbook/assets/image (288).png" alt="" width="482"><figcaption></figcaption></figure>

3. (Optional) If the data has already been stored in a vector database without going through the document store indexing pipeline, you can also connect directly to the vector database and embedding model.

<figure><img src="../.gitbook/assets/image (289).png" alt="" width="388"><figcaption></figcaption></figure>

4. Add a system prompt, or use the **Generate** button to assist. We recommend using it, as it helps craft a more effective and optimized prompt.

<figure><img src="../.gitbook/assets/image (294).png" alt="" width="482"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (292).png" alt="" width="563"><figcaption></figcaption></figure>

5. Your RAG agent is now ready to use!

## Resources

{% embed url="https://youtu.be/KHc0ClOIv0A?si=mEZJydM8bT2imKJY" %}
