# Pinecone

## Prerequisite

1. Register an account for [Pinecone](https://www.pinecone.io/)
2. Click **Create your first index**

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

3. Fill in required fields:
   * **Index Name**, name of the index to be created. (e.g. elon-musk)
   * **Dimensions**, size of the vectors to be inserted in the index. (e.g. 1536)

<figure><img src="../../../.gitbook/assets/image (33).png" alt="" width="527"><figcaption></figcaption></figure>

4. Click **Create Index**

## Setup

1.  Get/Create your **API Key** and **Environment**\


    <figure><img src="../../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>
2. Add a new Pinecone node to canvas and fill in the parameters:
   * Pinecone Index
   * Pinecone namespace (optional)

<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt="" width="279"><figcaption></figcaption></figure>

3. Create new Pinecone credential -> Fill in **API Key** and **Environment**

<figure><img src="../../../.gitbook/assets/image (2) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

4. Start the upsert process:

<figure><img src="../../../.gitbook/assets/Untitled.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

5. Verify from Pinecone dashboard to see if data has been successfully upserted:

<figure><img src="../../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. **Document** can be connected with any node under [**Document Loader**](../document-loaders/) category
7. **Embeddings** can be connected with any node under [**Embeddings** ](../embeddings/)category

## Resources

* [LangChain JS Pinecone](https://js.langchain.com/docs/modules/indexes/vector\_stores/integrations/pinecone)
* [Pinecone Node.JS](https://docs.pinecone.io/docs/node-client)
