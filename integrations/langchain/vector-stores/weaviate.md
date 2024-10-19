---
description: >-
  Upsert embedded data and perform similarity or mmr search using Weaviate, a
  scalable open-source vector database.
---

# Weaviate

<figure><img src="../../../.gitbook/assets/image (165).png" alt="" width="295"><figcaption><p>Weaviate Node</p></figcaption></figure>

## Filtering

Weaviate supports following [syntax](https://weaviate.io/developers/weaviate/search/filters) when it comes to filtering:

**UI**

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt="" width="227"><figcaption></figcaption></figure>

**API**

```json
"overrideConfig": {
    "weaviateFilter": {
        "where": {
            "operator": "Equal",
            "path": [
                "test"
            ],
            "valueText": "key"
        }
    }
}
```

## Resources

* [LangchainJS Weaviate](https://js.langchain.com/v0.1/docs/integrations/vectorstores/weaviate/#usage-query-documents)
* [Weaviate Filtering](https://weaviate.io/developers/weaviate/search/filters)

{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](../../../contributing/) to get started.
{% endhint %}
