---
description: >-
  Upsert embedded data and perform similarity or mmr search upon query using
  MongoDB Atlas, a managed cloud mongodb database.
---

# MongoDB Atlas

<figure><img src="../../../.gitbook/assets/image (161).png" alt="" width="308"><figcaption><p>MongoDB Atlas Node</p></figcaption></figure>

### Cluster Configuration[​](https://js.langchain.com/docs/integrations/vectorstores/mongodb\_atlas/#initial-cluster-configuration) <a href="#initial-cluster-configuration" id="initial-cluster-configuration"></a>

To set up a MongoDB Atlas cluster, go to the [MongoDB Atlas ](https://www.mongodb.com/)website and sign up if you don’t have an account. When prompted, create and name your cluster, which will appear under the Database section. Then, select "**Browse Collections**" to either create a new collection or use one from the sample data provided.

{% hint style="warning" %}
Ensure the cluster you create is version 7.0 or higher.
{% endhint %}

### Creating Index

After setting up your cluster, the next step is to create an index for the collection field you intend to search.

1. Go to the **Atlas Search** tab and click on **Create Search Index**.
2. Select **Atlas Vector Search - JSON Editor**, choose the appropriate database and collection, and then paste the following into the text box:

```json
{
  "fields": [
    {
      "numDimensions": 1536,
      "path": "embedding",
      "similarity": "euclidean",
      "type": "vector"
    }
  ]
}
```

Make sure the `numDimensions` property corresponds to the dimensionality of the embeddings you're using. For instance, Cohere embeddings typically have 1024 dimensions, while OpenAI embeddings have 1536 by default.

**Note:** The vector store expects certain default values, such as:

* An index name of `default`
* A collection field name of `embedding`
* A raw text field name of `text`

Ensure you initialize the vector store with field names that match your index and collection schema, as shown in the example above.

Once this is done, proceed to build the index.

{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](../../../contributing/) to get started.
{% endhint %}

### Flowise Configuration

Drag and drop the MongoDB Atlas Vector Store, and add a new credential. Use the connection string provided from the MongoDB Atlas dashboard:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Fill in the rest of the fields:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt="" width="252"><figcaption></figcaption></figure>

You may also configure more details from Additional Parameters:

<figure><img src="../../../.gitbook/assets/image (164).png" alt="" width="518"><figcaption></figcaption></figure>
