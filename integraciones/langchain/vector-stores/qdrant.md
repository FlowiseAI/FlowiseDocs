# Qdrant

## Prerequisitos

Una [instancia local de Qdrant](https://qdrant.tech/documentation/quick-start/) o una instancia de Qdrant cloud.

Para obtener una instancia de Qdrant cloud:

1. Dirígete a la sección Clusters del [Cloud Dashboard](https://cloud.qdrant.io/overview).
2. Selecciona **Clusters** y luego haz clic en **+ Create**.

<figure><img src="../../../.gitbook/assets/qdrant/2.png" alt=""><figcaption></figcaption></figure>

3. Elige las configuraciones de tu cluster y la región.
4. Presiona **Create** para aprovisionar tu cluster.

## Configuración

1. Obtén/Crea tu **API Key** desde la sección **Data Access Control** del [Cloud Dashboard](https://cloud.qdrant.io/overview).
2. Añade un nuevo nodo **Qdrant** en el canvas.
3. Crea una nueva credencial de Qdrant usando el API Key

<figure><img src="../../../.gitbook/assets/qdrant/1.png" alt="" width="563"><figcaption></figcaption></figure>

4. Ingresa la información requerida en el nodo **Qdrant**:
   * URL del servidor Qdrant
   * Nombre de la colección

<figure><img src="../../../.gitbook/assets/qdrant/3.png" alt="" width="239"><figcaption></figcaption></figure>

5. La entrada **Document** puede conectarse con cualquier nodo de la categoría [**Document Loader**](../document-loaders/).
6. La entrada **Embeddings** puede conectarse con cualquier nodo de la categoría [**Embeddings**](../embeddings/).

## Filtrado

Supongamos que tienes diferentes documentos insertados, cada uno especificado con un valor único bajo la clave de metadata `{source}`

<div align="left">

<figure><img src="../../../.gitbook/assets/Screenshot 2024-03-05 141551.png" alt="" width="563"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Screenshot 2024-03-05 141619.png" alt="" width="563"><figcaption></figcaption></figure>

</div>

Entonces, quieres filtrar por este valor. Qdrant soporta la siguiente [sintaxis](https://qdrant.tech/documentation/concepts/filtering/#nested-key) cuando se trata de filtrado:

**UI**

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2) (1) (1) (1).png" alt="" width="338"><figcaption></figcaption></figure>

**API**

```json
"overrideConfig": {
    "qdrantFilter": {
        "should": [
            {
                "key": "metadata.source",
                "match": {
                    "value": "apple"
                }
            }
        ]
    }
}
```

## Recursos

* [Documentación de Qdrant](https://qdrant.tech/documentation/)
* [LangChain JS Qdrant](https://js.langchain.com/docs/integrations/vectorstores/qdrant)
* [Filtros de Qdrant](https://qdrant.tech/documentation/concepts/filtering/#nested-key)
