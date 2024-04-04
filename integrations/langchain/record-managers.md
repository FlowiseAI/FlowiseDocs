# Record Managers

Record Managers keep track of your indexed documents, preventing duplicated vector embeddings in [Vector Store](vector-stores/).

When document chunks are upserting, each chunk will be hashed using [SHA-1](https://github.com/emn178/js-sha1) algorithm. These hashes will get stored in Record Manager. If there is an existing hash, the embedding and upserting process will be skipped.

In some cases, you might want to delete existing documents that are derived from the same sources as the new documents being indexed. For that, there are 3 cleanup modes for Record Manager:

<details>

<summary>None</summary>

No cleanup will be performed

</details>

<details>

<summary>Incremental</summary>

When you are upserting multiple documents, and you want to prevent deletion of the existing documents that are not part of the current upserting process, use **Incremental** Cleanup mode.

1. Let's have a Record Manager with `Incremental` Cleanup and `source` as SourceId Key

![](<../../.gitbook/assets/image (4).png>)<img src="../../.gitbook/assets/image (5).png" alt="" data-size="original">

2. And have the following 2 documents:
   *   1st document:

       Text: **Cat**, Metadata: `{ source: "cat" }`
   *   2nd document:

       Text: **Dog**, Metadata: `{ source: "dog" }`

![](<../../.gitbook/assets/image (11).png>)<img src="../../.gitbook/assets/image (8).png" alt="" data-size="original">

![](<../../.gitbook/assets/image (2) (1).png>)![](<../../.gitbook/assets/image (3).png>)

3. After an upsert, we will see 2 documents that are upserted:

![](<../../.gitbook/assets/image (12).png>)

4. Now, if we delete the **Dog** document, and update **Cat** to **Cats**, we will now see the following:

![](<../../.gitbook/assets/image (13).png>)

* The original **Cat** document is deleted
* A new document with **Cats** is added
* **Dog** document is left untouched

5. The remaining vector embeddings in Vector Store are **Cats** and **Dog**

![](<../../.gitbook/assets/image (15).png>)

</details>

<details>

<summary>Full</summary>

When you are upserting multiple documents, **Full** Cleanup mode will automatically delete any vector embeddings that are not part of the current upserting process.

1. Let's have a Record Manager with `Full`Cleanup. We don't need to have a SourceId Key for Full or None Cleanup mode.

![](<../../.gitbook/assets/image (4).png>)![](<../../.gitbook/assets/image (17).png>)

2. And have the following 2 documents:
   *   1st document:

       Text: **Cat**, Metadata: `{ source: "cat" }`
   *   2nd document:

       Text: **Dog**, Metadata: `{ source: "dog" }`

![](<../../.gitbook/assets/image (11).png>)<img src="../../.gitbook/assets/image (8).png" alt="" data-size="original">

![](<../../.gitbook/assets/image (2) (1).png>)![](<../../.gitbook/assets/image (3).png>)

3. After an upsert, we will see 2 documents that are upserted:

![](<../../.gitbook/assets/image (12).png>)

4. Now, if we delete the **Dog** document, and update **Cat** to **Cats**, we will now see the following:

![](<../../.gitbook/assets/image (18).png>)

* The original **Cat** document is deleted
* A new document with **Cats** is added
* **Dog** document is deleted

5. The remaining vector embeddings in Vector Store is only **Cats:**

![](<../../.gitbook/assets/image (19).png>)

</details>



{% tabs %}
{% tab title="Incremental" %}
When you are upserting multiple documents, and you want to prevent deletion of the existing documents that are not part of the current upserting process, use **Incremental** Cleanup mode.

1. Let's have a Record Manager with `Incremental` Cleanup and `source` as SourceId Key

<div align="left">

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="264"><figcaption></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/image (5).png" alt="" width="410"><figcaption></figcaption></figure>

</div>

2. And have the following 2 documents:

| Text | Metadata         |
| ---- | ---------------- |
| Cat  | `{source:"cat"}` |
| Dog  | `{source:"dog"}` |

<div align="left">

<figure><img src="../../.gitbook/assets/image (11).png" alt="" width="202"><figcaption></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/image.png" alt="" width="563"><figcaption></figcaption></figure>

</div>

<div align="left">

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt="" width="231"><figcaption></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

</div>

3. After an upsert, we will see 2 documents that are upserted:

<figure><img src="../../.gitbook/assets/image (9).png" alt="" width="433"><figcaption></figcaption></figure>

4. Now, if we delete the **Dog** document, and update **Cat** to **Cats**, we will now see the following:

<figure><img src="../../.gitbook/assets/image (13).png" alt="" width="425"><figcaption></figcaption></figure>

* The original **Cat** document is deleted
* A new document with **Cats** is added
* **Dog** document is left untouched



5. The remaining vector embeddings in Vector Store are **Cats** and **Dog**

<figure><img src="../../.gitbook/assets/image (15).png" alt="" width="448"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Full" %}
When you are upserting multiple documents, **Full** Cleanup mode will automatically delete any vector embeddings that are not part of the current upserting process.

1. Let's have a Record Manager with `Full` Cleanup. We don't need to have a SourceId Key for Full Cleanup mode.

<div align="left">

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="264"><figcaption></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/image (17).png" alt="" width="407"><figcaption></figcaption></figure>

</div>

2. And have the following 2 documents:

| Text | Metadata         |
| ---- | ---------------- |
| Cat  | `{source:"cat"}` |
| Dog  | `{source:"dog"}` |

<div align="left">

<figure><img src="../../.gitbook/assets/image (11).png" alt="" width="202"><figcaption></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/image.png" alt="" width="563"><figcaption></figcaption></figure>

</div>

<div align="left">

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt="" width="231"><figcaption></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

</div>

3. After an upsert, we will see 2 documents that are upserted:

<figure><img src="../../.gitbook/assets/image (9).png" alt="" width="433"><figcaption></figcaption></figure>

4. Now, if we delete the **Dog** document, and update **Cat** to **Cats**, we will now see the following:

<figure><img src="../../.gitbook/assets/image (18).png" alt="" width="430"><figcaption></figcaption></figure>

* The original **Cat** document is deleted
* A new document with **Cats** is added
* **Dog** document is deleted



5. The remaining vector embeddings in Vector Store is just **Cats**

<figure><img src="../../.gitbook/assets/image (19).png" alt="" width="527"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="None" %}
No cleanup will be performed
{% endtab %}
{% endtabs %}

Current available Record Managers are:

* SQLite
* MySQL
* PostgresQL

## Resources

* [Langchain Indexing](https://js.langchain.com/docs/modules/data\_connection/indexing/)
