# Qdrant

## Prerequisites

A [locally running instance of Qdrant](https://qdrant.tech/documentation/quick-start/) or a Qdrant cloud instance.

To get a Qdrant cloud instance:

1. Head to the Clusters section of the [Cloud Dashboard](https://cloud.qdrant.io/overview).
2. Select **Clusters** and then click **+ Create**.

<figure><img src="../../../.gitbook/assets/qdrant/2.png" alt=""><figcaption></figcaption></figure>

3. Choose your cluster configurations and region.
4. Hit **Create** to provision your cluster.

## Setup

1. Get/Create your **API Key** from the **Data Access Control** section of the [Cloud Dashboard](https://cloud.qdrant.io/overview).
2. Add a new **Qdrant** node on canvas.
3. Create new Qdrant credential using the API Key

<figure><img src="../../../.gitbook/assets/qdrant/1.png" alt="" width="563"><figcaption></figcaption></figure>

4. Enter the required info into the **Qdrant** node:
   * Qdrant server URL
   * Collection name

<figure><img src="../../../.gitbook/assets/qdrant/3.png" alt="" width="239"><figcaption></figcaption></figure>

5. **Document** input can be connected with any node under [**Document Loader**](../document-loaders/) category.
6. **Embeddings** input can be connected with any node under [**Embeddings**](../embeddings/)category.

## Resources

* [Qdrant documentation](https://qdrant.tech/documentation/)
* [LangChain JS Qdrant](https://js.langchain.com/docs/integrations/vectorstores/qdrant)
