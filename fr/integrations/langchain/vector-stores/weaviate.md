---
description: >-
  Upsert embedded data and perform similarity or mmr search using Weaviate, a
  scalable open-source vector database.
---

# Tisser

<gigne> <img src = "../../../. GitBook / Assets / Image (165) .png" alt = "" width = "295"> <Figcaption> <p> Tempsser le nœud </p> </gigcaption> </ Figure>

## Filtration

Terminer les supports suivants[syntax](https://weaviate.io/developers/weaviate/search/filters)En ce qui concerne le filtrage:

** ui **

<gigne> <img src = "../../../. GitBook / Assets / Image (5) (1) (1) (2) .png" alt = "" width = "227"> <Figcaption> </ Figcaption> </ Figure>

** API **

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

## Ressources

* [LangchainJS Weaviate](https://js.langchain.com/v0.1/docs/integrations/vectorstores/weaviate/#usage-query-documents)
* [Weaviate Filtering](https://weaviate.io/developers/weaviate/search/filters)

{% hint style = "info"%}
Cette section est un travail en cours. Nous apprécions toute aide que vous pouvez fournir pour terminer cette section. Veuillez vérifier notre[Contribution Guide](broken-reference)Pour commencer.
{% EndHint%}
