# Elastic

## Prerequisite

1. You can use the [official Docker image](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html) to get started, or you can use [Elastic Cloud](https://www.elastic.co/cloud/), Elastic's official cloud service. In this guide, we will be using cloud version.
2. [Register](https://cloud.elastic.co/registration) an account or [login](https://cloud.elastic.co/login) with existing account on Elastic cloud.

<figure><img src="../../../.gitbook/assets/elastic1.png" alt=""><figcaption></figcaption></figure>

3. Click **Create deployment**. Then, name your deployment, and choose the provider.

<figure><img src="../../../.gitbook/assets/elastic2.png" alt="" width="563"><figcaption></figcaption></figure>

4. After deployment is finished, you should be able to see the setup guides as shown below. Click the **Set up vector search** option.

<figure><img src="../../../.gitbook/assets/elastic4.png" alt=""><figcaption></figcaption></figure>

5. You should now see the **Getting started** page for **Vector Search**.

<figure><img src="../../../.gitbook/assets/elastic5.png" alt=""><figcaption></figcaption></figure>

6. On the left hand side bar, click **Indices**. Then, **Create a new index**.

<figure><img src="../../../.gitbook/assets/elastic6.png" alt=""><figcaption></figcaption></figure>

7. Select **API** ingestion method

<figure><img src="../../../.gitbook/assets/elastic7.png" alt=""><figcaption></figcaption></figure>

8. Name your search index name, then **Create Index**

<figure><img src="../../../.gitbook/assets/elastic8.png" alt=""><figcaption></figcaption></figure>

9. After the index has been created, generate a new API key, take note of both generated API key and the URL

<figure><img src="../../../.gitbook/assets/elastic9.png" alt=""><figcaption></figcaption></figure>

## Flowise Setup

1. Add a new **Elasticsearch** node on canvas and fill in the **Index Name**

<figure><img src="../../../.gitbook/assets/elastic10.png" alt="" width="275"><figcaption></figcaption></figure>

2. Add new credential via **Elasticsearch API**

<figure><img src="../../../.gitbook/assets/elastic11.png" alt="" width="429"><figcaption></figcaption></figure>

3. Take the URL and API Key from Elasticsearch, fill in the fields

<figure><img src="../../../.gitbook/assets/elastic12.png" alt="" width="563"><figcaption></figcaption></figure>

4. After credential has been created successfully, you can start upserting the data

<figure><img src="../../../.gitbook/assets/Untitled (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/elastic13.png" alt=""><figcaption></figcaption></figure>

5. After data has been upserted successfully, you can verify it from Elastic dashboard:

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

6. Voila! You can now start asking question in the chat

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

## Resources

* [LangChain JS Elastic](https://js.langchain.com/docs/integrations/vectorstores/elasticsearch)
* [Vector Search (kNN) Implementation Guide - API Edition](https://www.elastic.co/search-labs/blog/articles/vector-search-implementation-guide-api-edition)
