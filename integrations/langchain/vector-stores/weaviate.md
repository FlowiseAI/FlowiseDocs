---
description: >-
  Realiza upsert de datos embedidos y ejecuta búsquedas de similitud o mmr usando Weaviate,
  una base de datos vectorial escalable de código abierto.
---

# Weaviate

<figure><img src="../../../.gitbook/assets/image (165).png" alt="" width="295"><figcaption><p>Nodo Weaviate</p></figcaption></figure>

## Filtrado

Weaviate soporta la siguiente [sintaxis](https://weaviate.io/developers/weaviate/search/filters) cuando se trata de filtrado:

**UI**

<figure><img src="../../../.gitbook/assets/image (5) (1) (1).png" alt="" width="227"><figcaption></figcaption></figure>

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

## Recursos

* [LangchainJS Weaviate](https://js.langchain.com/v0.1/docs/integrations/vectorstores/weaviate/#usage-query-documents)
* [Filtrado en Weaviate](https://weaviate.io/developers/weaviate/search/filters)

{% hint style="info" %}
Esta sección está en desarrollo. Agradecemos cualquier ayuda que puedas proporcionar para completar esta sección. Por favor, consulta nuestra [Guía de Contribución](../../../contributing/) para comenzar.
{% endhint %}
