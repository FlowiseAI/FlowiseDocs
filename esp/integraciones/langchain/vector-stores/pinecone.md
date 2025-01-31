---
description: >-
  Realiza upsert de datos embedidos y ejecuta búsquedas de similitud sobre consultas usando Pinecone,
  una base de datos vectorial gestionada líder en el mercado.
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

1. Obtén/Crea tu **API Key**

<figure><img src="../../../.gitbook/assets/pinecone_3.png" alt=""><figcaption></figcaption></figure>

2. Agrega un nuevo nodo **Pinecone** al canvas y completa los parámetros:
   - Pinecone Index
   - Pinecone namespace (opcional)

<figure><img src="../../../.gitbook/assets/pinecone_4.png" alt="" width="279"><figcaption></figcaption></figure>

3. Crea una nueva credencial de Pinecone -> Completa el **API Key**

<figure><img src="../../../.gitbook/assets/pinecone_5.png" alt="" width="563"><figcaption></figcaption></figure>

4. Agrega nodos adicionales al canvas e inicia el proceso de upsert
   - **Document** puede conectarse con cualquier nodo bajo la categoría [**Document Loader**](../document-loaders/)
   - **Embeddings** puede conectarse con cualquier nodo bajo la categoría [**Embeddings**](../embeddings/)

<figure><img src="../../../.gitbook/assets/pinecone_6.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/pinecone_7.png" alt=""><figcaption></figcaption></figure>

5. Verifica desde el [dashboard de Pinecone](https://app.pinecone.io) si los datos se han insertado correctamente:

<figure><img src="../../../.gitbook/assets/pinecone_8.png" alt=""><figcaption></figcaption></figure>

## Recursos

- Integraciones de vectorstore de LangChain con Pinecone
  - [Python](https://python.langchain.com/v0.2/docs/integrations/providers/pinecone/)
  - [NodeJS](https://js.langchain.com/v0.2/docs/integrations/vectorstores/pinecone)
- [Integración de Pinecone con LangChain](https://docs.pinecone.io/integrations/langchain)
- [Integración de Pinecone con Flowise](https://docs.pinecone.io/integrations/flowise)
- [Clientes oficiales de Pinecone](https://docs.pinecone.io/reference/pinecone-clients)
