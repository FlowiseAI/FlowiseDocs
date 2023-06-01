# Pinecone

## Prerequisite

1. Register an account for [Pinecone](https://www.pinecone.io/)
2.  Click **Create your first index**\


    <figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>
3.  Input required fields\
    **Index Name**, name of the index to be created. (e.g. elon-musk)\
    **Dimensions**, size of the vectors to be inserted in the index. (e.g. 1536)\


    <figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>
4. Click **Create Index**

## Setup

1.  Get/Create your **API Key**\
    &#x20;

    <figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>
2. Copy & Paste each details (_API Key, Environment, Index Name_) into _**Pinecone Upsert Document**_ node or **Pinecone Load Existing Index** node\
   ![](<../.gitbook/assets/image (39).png>)![](<../.gitbook/assets/image (12).png>)
3. **Document** can be connect with any node under [**Document Loader**](../document-loaders.md) category
4. **Embeddings** can be connect with any node under [**Embeddings** ](../embeddings.md)category

## Resources

* [LangChain JS Pinecone](https://js.langchain.com/docs/modules/indexes/vector\_stores/integrations/pinecone)
* [Pinecone Node.JS](https://docs.pinecone.io/docs/node-client)
