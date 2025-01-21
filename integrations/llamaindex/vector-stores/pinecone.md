---
description: >-
  Realiza upsert de datos embedidos y ejecuta búsquedas de similitud usando Pinecone,
  una base de datos vectorial gestionada y alojada líder en el mercado.
---

# Pinecone

## Prerequisitos

1. Registra una cuenta en [Pinecone](https://app.pinecone.io/)
2. Haz clic en **Create index**

<figure><img src="../../../.gitbook/assets/pinecone_1.png" alt=""><figcaption></figcaption></figure>

3. Completa los campos requeridos:
   - **Index Name**, nombre del índice a crear. (por ejemplo, "flowise-test")
   - **Dimensions**, tamaño de los vectores a insertar en el índice. (por ejemplo, 1536)

<figure><img src="../../../.gitbook/assets/pinecone_2.png" alt="" width="527"><figcaption></figcaption></figure>

4. Haz clic en **Create Index**

## Configuración

1.  Obtén/Crea tu **API Key**

<figure><img src="../../../.gitbook/assets/pinecone_3.png" alt=""><figcaption></figcaption></figure>

2.  Añade un nuevo nodo **Pinecone** al canvas y completa los parámetros:
    - Pinecone Index
    - Pinecone namespace (opcional)

<figure><img src="../../../.gitbook/assets/pinecone_llamaindex.png" alt="" width="301"><figcaption><p>Nodo Pinecone</p></figcaption></figure>

3. Crea una nueva credencial de Pinecone -> Completa el **API Key**

<figure><img src="../../../.gitbook/assets/pinecone_5.png" alt="" width="563"><figcaption></figcaption></figure>

4. Añade nodos adicionales al canvas e inicia el proceso de upsert
   - **Document** puede conectarse con cualquier nodo bajo la categoría [**Document Loader**](../../langchain/document-loaders/)
     {% hint style="info" %}
     Los document loaders y text splitters para LlamaIndex aún no están disponibles, pero usar uno de los disponibles en LangChain permitirá realizar consultas con LlamaIndex de manera normal.
     {% endhint %}
   - **Embeddings** puede conectarse con cualquier nodo bajo la categoría [**Embeddings**](../embeddings/)

<figure><img src="../../../.gitbook/assets/pinecone_llama_chatflow.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/pinecone_llama_upsert.png" alt=""><figcaption></figcaption></figure>

5. Verifica en el [dashboard de Pinecone](https://app.pinecone.io) que los datos se han insertado correctamente:

<figure><img src="../../../.gitbook/assets/pinecone_8.png" alt=""><figcaption></figcaption></figure>
