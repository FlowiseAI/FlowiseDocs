# Elastic

## Prerequisitos

1. Puedes usar la [imagen oficial de Docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html) para empezar, o puedes usar [Elastic Cloud](https://www.elastic.co/cloud/), el servicio cloud oficial de Elastic. En esta guía, usaremos la versión cloud.
2. [Registra](https://cloud.elastic.co/registration) una cuenta o [inicia sesión](https://cloud.elastic.co/login) con una cuenta existente en Elastic cloud.

<figure><img src="../../../.gitbook/assets/elastic1.png" alt=""><figcaption></figcaption></figure>

3. Haz clic en **Create deployment**. Luego, nombra tu deployment y elige el proveedor.

<figure><img src="../../../.gitbook/assets/elastic2.png" alt="" width="563"><figcaption></figcaption></figure>

4. Después de que el deployment haya finalizado, deberías poder ver las guías de configuración como se muestra a continuación. Haz clic en la opción **Set up vector search**.

<figure><img src="../../../.gitbook/assets/elastic4.png" alt=""><figcaption></figcaption></figure>

5. Ahora deberías ver la página de **Getting started** para **Vector Search**.

<figure><img src="../../../.gitbook/assets/elastic5.png" alt=""><figcaption></figcaption></figure>

6. En la barra lateral izquierda, haz clic en **Indices**. Luego, **Create a new index**.

<figure><img src="../../../.gitbook/assets/elastic6.png" alt=""><figcaption></figcaption></figure>

7. Selecciona el método de ingesta **API**

<figure><img src="../../../.gitbook/assets/elastic7.png" alt=""><figcaption></figcaption></figure>

8. Nombra tu índice de búsqueda, luego **Create Index**

<figure><img src="../../../.gitbook/assets/elastic8.png" alt=""><figcaption></figcaption></figure>

9. Después de que el índice haya sido creado, genera una nueva API key, toma nota tanto de la API key generada como de la URL

<figure><img src="../../../.gitbook/assets/elastic9.png" alt=""><figcaption></figcaption></figure>

## Configuración en Flowise

1. Agrega un nuevo nodo **Elasticsearch** en el canvas y completa el **Index Name**

<figure><img src="../../../.gitbook/assets/elastic10.png" alt="" width="275"><figcaption></figcaption></figure>

2. Agrega una nueva credencial vía **Elasticsearch API**

<figure><img src="../../../.gitbook/assets/elastic11.png" alt="" width="429"><figcaption></figcaption></figure>

3. Toma la URL y API Key de Elasticsearch, completa los campos

<figure><img src="../../../.gitbook/assets/elastic12.png" alt="" width="563"><figcaption></figcaption></figure>

4. Después de que la credencial se haya creado exitosamente, puedes comenzar a hacer upsert de los datos

<figure><img src="../../../.gitbook/assets/Untitled (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/elastic13.png" alt=""><figcaption></figcaption></figure>

5. Después de que los datos se hayan insertado exitosamente, puedes verificarlo desde el dashboard de Elastic:

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

6. ¡Voilà! Ahora puedes comenzar a hacer preguntas en el chat

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

## Recursos

* [LangChain JS Elastic](https://js.langchain.com/docs/integrations/vectorstores/elasticsearch)
* [Vector Search (kNN) Implementation Guide - API Edition](https://www.elastic.co/search-labs/blog/articles/vector-search-implementation-guide-api-edition)
