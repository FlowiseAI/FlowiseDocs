# Upstash

## Prerequisitos

1. Regístrate o inicia sesión en [Upstash Console](https://console.upstash.com)
2. Navega a la página Vector y haz clic en **Create Index**
   <figure><img src="../../../.gitbook/assets/upstash/list-index.jpeg" alt=""><figcaption></figcaption></figure>
3. Realiza las configuraciones necesarias y crea el índice.

   1. **Index Name**, nombre del índice a crear (ej. "flowise-upstash-demo")
   2. **Dimensions**, tamaño de los vectores a insertar en el índice (ej. 1536)
   3. **Embedding Model**, el modelo a usar en [Upstash Embeddings](https://upstash.com/docs/vector/features/embeddingmodels). Esto es opcional. Si lo habilitas, no necesitas proporcionar un modelo de embeddings.

   <figure><img src="../../../.gitbook/assets/upstash/create-index.jpeg" alt=""><figcaption></figcaption></figure>

## Configuración

1. Obtén las credenciales de tu índice

<figure><img src="../../../.gitbook/assets/upstash/env-variables.jpeg" alt=""><figcaption></figcaption></figure>

2. Crea una nueva credencial de Upstash Vector y completa
   1. Upstash Vector REST URL desde UPSTASH_VECTOR_REST_URL en la consola
   2. Upstash Vector Rest Token desde UPSTASH_VECTOR_REST_TOKEN en la consola

<figure><img src="../../../.gitbook/assets/upstash/credentials.jpeg" alt="" width="563"><figcaption></figcaption></figure>

3. Añade un nuevo nodo **Upstash Vector** al canvas

<figure><img src="../../../.gitbook/assets/upstash/upstash-node.jpeg" alt="" width="279"><figcaption></figcaption></figure>

4. Añade nodos adicionales al canvas e inicia el proceso de upsert
   - **Document** puede conectarse con cualquier nodo de la categoría [**Document Loader**](../document-loaders/)
   - **Embeddings** puede conectarse con cualquier nodo de la categoría [**Embeddings**](../embeddings/)

<figure><img src="../../../.gitbook/assets/upstash/flowise-design.jpeg" alt=""><figcaption></figcaption></figure>

5. Verifica desde el [dashboard de Upstash](https://console.upstash.com) si los datos se han actualizado correctamente:

<figure><img src="../../../.gitbook/assets/upstash/databrowser.jpeg" alt=""><figcaption></figcaption></figure>
