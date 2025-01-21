---
description: Nodos Record Manager de LangChain
---

# Record Managers

***

Los Record Managers hacen un seguimiento de tus documentos indexados, evitando embeddings vectoriales duplicados en [Vector Store](vector-stores/).

Cuando se realizan upserts de fragmentos de documentos, cada fragmento será hasheado usando el algoritmo [SHA-1](https://github.com/emn178/js-sha1). Estos hashes se almacenarán en el Record Manager. Si existe un hash, el proceso de embedding y upsert será omitido.

En algunos casos, podrías querer eliminar documentos existentes que se derivan de las mismas fuentes que los nuevos documentos que se están indexando. Para eso, hay 3 modos de limpieza para Record Manager:

{% tabs %}
{% tab title="Incremental" %}
Cuando estás haciendo upsert de múltiples documentos y quieres evitar la eliminación de los documentos existentes que no son parte del proceso actual de upsert, usa el modo de limpieza **Incremental**.

1. Tengamos un Record Manager con modo de limpieza `Incremental` y `source` como SourceId Key

<div align="left"><figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="264"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="410"><figcaption></figcaption></figure></div>

2. Y tengamos los siguientes 2 documentos:

| Texto | Metadata         |
| ----- | ---------------- |
| Cat   | `{source:"cat"}` |
| Dog   | `{source:"dog"}` |

<div align="left"><figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1).png" alt="" width="202"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (2).png" alt="" width="231"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. Después de un upsert, veremos 2 documentos que se han insertado:

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2).png" alt="" width="433"><figcaption></figcaption></figure>

4. Ahora, si eliminamos el documento **Dog** y actualizamos **Cat** a **Cats**, veremos lo siguiente:

<figure><img src="../../.gitbook/assets/image (13) (2).png" alt="" width="425"><figcaption></figcaption></figure>

* El documento original **Cat** es eliminado
* Un nuevo documento con **Cats** es añadido
* El documento **Dog** no se modifica
* Los embeddings vectoriales restantes en Vector Store son **Cats** y **Dog**

<figure><img src="../../.gitbook/assets/image (15) (1) (1).png" alt="" width="448"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Full" %}
Cuando estás haciendo upsert de múltiples documentos, el modo de limpieza **Full** eliminará automáticamente cualquier embedding vectorial que no sea parte del proceso actual de upsert.

1. Tengamos un Record Manager con limpieza `Full`. No necesitamos tener un SourceId Key para el modo de limpieza Full.

<div align="left"><figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="264"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (17) (1) (1).png" alt="" width="407"><figcaption></figcaption></figure></div>

2. Y tengamos los siguientes 2 documentos:

| Texto | Metadata         |
| ----- | ---------------- |
| Cat   | `{source:"cat"}` |
| Dog   | `{source:"dog"}` |

<div align="left"><figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1).png" alt="" width="202"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (2).png" alt="" width="231"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. Después de un upsert, veremos 2 documentos que se han insertado:

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2).png" alt="" width="433"><figcaption></figcaption></figure>

4. Ahora, si eliminamos el documento **Dog** y actualizamos **Cat** a **Cats**, veremos lo siguiente:

<figure><img src="../../.gitbook/assets/image (18) (1) (1).png" alt="" width="430"><figcaption></figcaption></figure>

* El documento original **Cat** es eliminado
* Un nuevo documento con **Cats** es añadido
* El documento **Dog** es eliminado
* El único embedding vectorial restante en Vector Store es **Cats**

<figure><img src="../../.gitbook/assets/image (19) (1) (1).png" alt="" width="527"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="None" %}
No se realizará ninguna limpieza
{% endtab %}
{% endtabs %}

Los nodos Record Manager actualmente disponibles son:

* SQLite
* MySQL
* PostgresQL

## Recursos

* [LangChain Indexing - Cómo funciona](https://js.langchain.com/docs/how_to/indexing/#how-it-works)
