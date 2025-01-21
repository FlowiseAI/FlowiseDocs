---
description: >-
  Realiza upsert de datos embedidos y ejecuta búsquedas de similitud o mmr sobre consultas usando
  MongoDB Atlas, una base de datos mongodb gestionada en la nube.
---

# MongoDB Atlas

<figure><img src="../../../.gitbook/assets/image (161).png" alt="" width="308"><figcaption><p>Nodo MongoDB Atlas</p></figcaption></figure>

### Configuración del Cluster[​](https://js.langchain.com/docs/integrations/vectorstores/mongodb_atlas/#initial-cluster-configuration) <a href="#initial-cluster-configuration" id="initial-cluster-configuration"></a>

Para configurar un cluster de MongoDB Atlas, ve al sitio web de [MongoDB Atlas ](https://www.mongodb.com/)y regístrate si no tienes una cuenta. Cuando se te solicite, crea y nombra tu cluster, que aparecerá en la sección Database. Luego, selecciona "**Browse Collections**" para crear una nueva collection o usar una de los datos de ejemplo proporcionados.

{% hint style="warning" %}
Asegúrate de que el cluster que crees sea versión 7.0 o superior.
{% endhint %}

### Creando el Index

Después de configurar tu cluster, el siguiente paso es crear un index para el campo de la collection que pretendes buscar.

1. Ve a la pestaña **Atlas Search** y haz clic en **Create Search Index**.
2. Selecciona **Atlas Vector Search - JSON Editor**, elige la base de datos y collection apropiada, y luego pega lo siguiente en el cuadro de texto:

```json
{
  "fields": [
    {
      "numDimensions": 1536,
      "path": "embedding",
      "similarity": "euclidean",
      "type": "vector"
    }
  ]
}
```

Asegúrate de que la propiedad `numDimensions` corresponda a la dimensionalidad de los embeddings que estás usando. Por ejemplo, los embeddings de Cohere típicamente tienen 1024 dimensiones, mientras que los embeddings de OpenAI tienen 1536 por defecto.

**Nota:** El vector store espera ciertos valores por defecto, como:

* Un nombre de index de `default`
* Un nombre de campo de collection de `embedding`
* Un nombre de campo de texto sin procesar de `text`

Asegúrate de inicializar el vector store con nombres de campos que coincidan con tu esquema de index y collection, como se muestra en el ejemplo anterior.

Una vez hecho esto, procede a construir el index.

{% hint style="info" %}
Esta sección está en desarrollo. Agradecemos cualquier ayuda que puedas proporcionar para completar esta sección. Por favor, consulta nuestra [Guía de Contribución](../../../contributing/) para comenzar.
{% endhint %}

### Configuración en Flowise

Arrastra y suelta el MongoDB Atlas Vector Store, y agrega una nueva credencial. Usa la cadena de conexión proporcionada desde el dashboard de MongoDB Atlas:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Completa el resto de los campos:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="252"><figcaption></figcaption></figure>

También puedes configurar más detalles desde Additional Parameters:

<figure><img src="../../../.gitbook/assets/image (164).png" alt="" width="518"><figcaption></figcaption></figure>
