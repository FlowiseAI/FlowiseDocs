---
description: LangChain Record Manager Nodes
---

# Record Managers

***

Record Managers keep track of your indexed documents, preventing duplicated vector embeddings in [Vector Store](vector-stores/).

When document chunks are upserting, each chunk will be hashed using [SHA-1](https://github.com/emn178/js-sha1) algorithm. These hashes will get stored in Record Manager. If there is an existing hash, the embedding and upserting process will be skipped.

In some cases, you might want to delete existing documents that are derived from the same sources as the new documents being indexed. For that, there are 3 cleanup modes for Record Manager:

{% tabs %}
{% tab title="Incremental" %}
When you are upserting multiple documents, and you want to prevent deletion of the existing documents that are not part of the current upserting process, use **Incremental** Cleanup mode.

1. Let's have a Record Manager with `Incremental` Cleanup and `source` as SourceId Key

<div align="left"><figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="264"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="410"><figcaption></figcaption></figure></div>

2. And have the following 2 documents:

| Text | Metadata         |
| ---- | ---------------- |
| Cat  | `{source:"cat"}` |
| Dog  | `{source:"dog"}` |

<div align="left"><figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="202"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (2).png" alt="" width="231"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. After an upsert, we will see 2 documents that are upserted:

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2).png" alt="" width="433"><figcaption></figcaption></figure>

4. Now, if we delete the **Dog** document, and update **Cat** to **Cats**, we will now see the following:

<figure><img src="../../.gitbook/assets/image (13) (2) (2).png" alt="" width="425"><figcaption></figcaption></figure>

* The original **Cat** document is deleted
* A new document with **Cats** is added
* **Dog** document is left untouched
* The remaining vector embeddings in Vector Store are **Cats** and **Dog**

<figure><img src="../../.gitbook/assets/image (15) (1) (1) (1) (1) (1) (1).png" alt="" width="448"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Full" %}
When you are upserting multiple documents, **Full** Cleanup mode will automatically delete any vector embeddings that are not part of the current upserting process.

1. Let's have a Record Manager with `Full` Cleanup. We don't need to have a SourceId Key for Full Cleanup mode.

<div align="left"><figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="264"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (2).png" alt="" width="407"><figcaption></figcaption></figure></div>

2. And have the following 2 documents:

| Text | Metadata         |
| ---- | ---------------- |
| Cat  | `{source:"cat"}` |
| Dog  | `{source:"dog"}` |

<div align="left"><figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="202"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (2).png" alt="" width="231"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. After an upsert, we will see 2 documents that are upserted:

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2).png" alt="" width="433"><figcaption></figcaption></figure>

4. Now, if we delete the **Dog** document, and update **Cat** to **Cats**, we will now see the following:

<figure><img src="../../.gitbook/assets/image (18) (1) (1) (1) (2).png" alt="" width="430"><figcaption></figcaption></figure>

* The original **Cat** document is deleted
* A new document with **Cats** is added
* **Dog** document is deleted
* The remaining vector embeddings in Vector Store is just **Cats**

<figure><img src="../../.gitbook/assets/image (19) (1) (1) (1).png" alt="" width="527"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="None" %}
No cleanup will be performed
{% endtab %}
{% endtabs %}

Current available Record Manager nodes are:

* SQLite
* MySQL
* PostgresQL

## Resources

* [LangChain Indexing - How it works](https://js.langchain.com/docs/how_to/indexing/#how-it-works)
