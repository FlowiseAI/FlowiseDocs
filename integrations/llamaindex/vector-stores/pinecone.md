---
description: >-
  Upsert embedded data and perform similarity search upon query using Pinecone,
  a leading fully managed hosted vector database.
---

# Pinecone

## Prerequisite

1. Register an account for [Pinecone](https://app.pinecone.io/)
2. Click **Create index**

<figure><img src="../../../.gitbook/assets/pinecone_1.png" alt=""><figcaption></figcaption></figure>

3. Fill in required fields:
   - **Index Name**, name of the index to be created. (e.g. "flowise-demo")
   - **Dimensions**, size of the vectors to be inserted in the index. (e.g. 1536)

<figure><img src="../../../.gitbook/assets/pinecone_2.png" alt="" width="527"><figcaption></figcaption></figure>

4. Click **Create Index**

## Setup

1.  Get/Create your **API Key**

<figure><img src="../../../.gitbook/assets/pinecone_3.png" alt=""><figcaption></figcaption></figure>

2.  Add a new **Pinecone** node to canvas and fill in the parameters:
    - Pinecone Index
    - Pinecone namespace (optional)

<figure><img src="../../../.gitbook/assets/pinecone_llamaindex.png" alt="" width="301"><figcaption><p>Pinecone Node</p></figcaption></figure>

3. Create new Pinecone credential -> Fill in **API Key**

<figure><img src="../../../.gitbook/assets/pinecone_5.png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](../../../CONTRIBUTING.md) to get started.
{% endhint %}
