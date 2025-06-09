---
description: Aprende cómo configurar las environment variables para Flowise
---

# Environment Variables

Flowise soporta diferentes environment variables para configurar tu instancia. Puedes especificar las siguientes variables en el archivo `.env` dentro de la carpeta `packages/server`. Consulta el archivo [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/.env.example).

<table><thead><tr><th width="233">Variable</th><th width="219">Descripción</th><th width="104">Type</th><th>Default</th></tr></thead><tbody><tr><td>PORT</td><td>El puerto HTTP en el que se ejecuta Flowise</td><td>Number</td><td>3000</td></tr><tr><td>FLOWISE_FILE_SIZE_LIMIT</td><td>Tamaño máximo de archivo al subir</td><td>String</td><td><code>50mb</code></td></tr><tr><td>NUMBER_OF_PROXIES</td><td>Rate Limit Proxy</td><td>Number</td><td></td></tr><tr><td>CORS_ORIGINS</td><td>Los orígenes permitidos para todas las llamadas HTTP cross-origin</td><td>String</td><td></td></tr><tr><td>IFRAME_ORIGINS</td><td>Los orígenes permitidos para incrustar en iframe src</td><td>String</td><td></td></tr><tr><td>DISABLE_CHATFLOW_REUSE</td><td>Deshabilita el cacheo del flow, permitiendo que cada interacción del chatflow se ejecute desde cero</td><td>Boolean: <code>true</code> o <code>false</code></td><td></td></tr><tr><td>SHOW_COMMUNITY_NODES</td><td>Muestra los nodes creados por la comunidad</td><td>Boolean: <code>true</code> o <code>false</code></td><td></td></tr></tbody></table>

## Para Database

| Variable           | Descripción                                                      | Type                                       | Default                  |
| ------------------ | ---------------------------------------------------------------- | ------------------------------------------ | ------------------------ |
| DATABASE_TYPE     | Tipo de database para almacenar los datos de flowise             | Enum String: `sqlite`, `mysql`, `postgres` | `sqlite`                 |
| DATABASE_PATH     | Ubicación donde se guarda la database (Cuando DATABASE_TYPE es sqlite) | String                                     | `your-home-dir/.flowise` |
| DATABASE_HOST     | Host URL o dirección IP (Cuando DATABASE_TYPE no es sqlite)       | String                                     |                          |
| DATABASE_PORT     | Puerto de la database (Cuando DATABASE_TYPE no es sqlite)         | String                                     |                          |
| DATABASE_USER     | Username de la database (Cuando DATABASE_TYPE no es sqlite)       | String                                     |                          |
| DATABASE_PASSWORD | Password de la database (Cuando DATABASE_TYPE no es sqlite)       | String                                     |                          |
| DATABASE_NAME     | Nombre de la database (Cuando DATABASE_TYPE no es sqlite)         | String                                     |                          |
| DATABASE_SSL      | SSL de la database es requerido (Cuando DATABASE_TYPE no es sqlite) | Boolean: `true` o `false`                 | `false`                  |

## Para LangSmith Tracing

Flowise soporta [LangSmith](https://docs.smith.langchain.com/) tracing con las siguientes env variables:

<table><thead><tr><th width="247">Variable</th><th>Descripción</th><th width="151">Type</th></tr></thead><tbody><tr><td>LANGCHAIN_TRACING_V2</td><td>Activa o desactiva LangSmith tracing</td><td>Enum String: <code>true</code>, <code>false</code></td></tr><tr><td>LANGCHAIN_ENDPOINT</td><td>LangSmith endpoint</td><td>String</td></tr><tr><td>LANGCHAIN_API_KEY</td><td>LangSmith API Key</td><td>String</td></tr><tr><td>LANGCHAIN_PROJECT</td><td>Project para hacer trace en LangSmith</td><td>String</td></tr></tbody></table>

### Mira cómo conectar Flowise y LangSmith

{% embed url="https://youtu.be/BGGhRxS3AVQ" %}

## Para Built-In y External Dependencies

Por razones de seguridad, por defecto Tool Function solo permite ciertas dependencies. Es posible levantar esa restricción para módulos built-in y externos configurando las siguientes environment variables:

| Variable                      | Descripción                                          |        |
| ----------------------------- | ---------------------------------------------------- | ------ |
| TOOL_FUNCTION_BUILTIN_DEP  | Módulos built-in de NodeJS para usar en Tool Function | String |
| TOOL_FUNCTION_EXTERNAL_DEP | Módulos externos para usar en Tool Function        | String |

{% code title=".env" %}
```bash
# Permite el uso de todos los módulos builtin
TOOL_FUNCTION_BUILTIN_DEP=*

# Permite el uso de solo fs
TOOL_FUNCTION_BUILTIN_DEP=fs

# Permite el uso de solo crypto y fs
TOOL_FUNCTION_BUILTIN_DEP=crypto,fs

# Permite el uso de módulos npm externos
TOOL_FUNCTION_EXTERNAL_DEP=axios,moment
```
{% endcode %}

## Para Debugging y Logs

| Variable   | Descripción                         | Type                                             |                                |
| ---------- | ----------------------------------- | ------------------------------------------------ | ------------------------------ |
| DEBUG      | Imprime logs de los componentes     | Boolean                                          |                                |
| LOG_PATH  | Ubicación donde se guardan los archivos de log | String                                           | `Flowise/packages/server/logs` |
| LOG_LEVEL | Diferentes niveles de logs          | Enum String: `error`, `info`, `verbose`, `debug` | `info`                         |

`DEBUG`: si se establece en true, imprimirá logs en la terminal/consola:

<figure><img src="../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

`LOG_LEVEL`: Diferentes niveles de log para los loggers que se guardarán. Puede ser `error`, `info`, `verbose`, o `debug`. Por defecto está establecido en `info`, solo `logger.info` se guardará en los archivos de log. Si quieres tener detalles completos, establécelo en `debug`.

<figure><img src="../.gitbook/assets/image (2) (4).png" alt=""><figcaption><p><strong>server-requests.log.jsonl - registra cada request enviada a Flowise</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><strong>server.log - registra acciones generales en Flowise</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (4).png" alt=""><figcaption><p><strong>server-error.log - registra errores con stack trace</strong></p></figcaption></figure>

## Para Credentials

Flowise almacena tus API keys de terceros como credentials encriptadas usando una encryption key.

Por defecto, se generará una random encryption key al iniciar la aplicación y se almacenará en una ruta de archivo. Esta encryption key se recupera cada vez para descifrar las credentials utilizadas dentro de un chatflow. Por ejemplo, tu OpenAI API key, Pinecone API key, etc.

| Variable                      | Descripción                                                                  | Type   |                           |
| ----------------------------- | ---------------------------------------------------------------------------- | ------ | ------------------------- |
| SECRETKEY_PATH               | Ubicación donde se guarda la encryption key (usada para encriptar/desencriptar credentials) | String | `Flowise/packages/server` |
| FLOWISE_SECRETKEY_OVERWRITE | Encryption key a usar en lugar de la key almacenada en SECRETKEY_PATH       | String |                           |

Por algunas razones, a veces la encryption key podría regenerarse o la ruta almacenada cambió, esto causará errores como - <mark style="color:red;">Credentials could not be decrypted.</mark>

Para evitar esto, puedes establecer tu propia encryption key como `FLOWISE_SECRETKEY_OVERWRITE`, así la misma encryption key se usará cada vez. No hay restricción en el formato, puedes establecerla como cualquier texto que desees, o la misma que tu `FLOWISE_PASSWORD`.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
La Credential API Key devuelta desde la UI no tiene la misma longitud que tu Api Key original que has establecido. Este es un prefijo de cadena falso que previene el spoofing de red, por eso no devolvemos la Api Key a la UI. Sin embargo, la Api Key correcta será recuperada y usada durante tu interacción con el chatflow.
{% endhint %}

## Para Models

En algunos casos, podrías querer usar un modelo personalizado en los nodes existentes de Chat Model y LLM, o restringir el acceso a solo ciertos modelos.

Por defecto, Flowise obtiene la lista de modelos de [aquí](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/models.json). Sin embargo, el usuario puede crear su propio archivo `models.json` y especificar la ruta del archivo:

<table><thead><tr><th width="164">Variable</th><th width="196">Descripción</th><th width="78">Type</th><th>Default</th></tr></thead><tbody><tr><td>MODEL_LIST_CONFIG_JSON</td><td>Link para cargar la lista de modelos desde tu archivo de configuración <code>models.json</code></td><td>String</td><td><a href="https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json">https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json</a></td></tr></tbody></table>

## Para Storage

Flowise almacena los siguientes archivos bajo una carpeta de ruta local por defecto.

* Archivos subidos en [Document Loaders](../integrations/langchain/document-loaders/)/Document Store
* Subidas de imagen/audio desde el chat
* Imágenes/Archivos desde Assistant
* Archivos desde [Vector Upsert API](../using-flowise/api.md#vector-upsert-api)

El usuario puede especificar `STORAGE_TYPE` para usar AWS S3 o ruta local

<table><thead><tr><th width="227">Variable</th><th width="196">Descripción</th><th width="131">Type</th><th>Default</th></tr></thead><tbody><tr><td>STORAGE_TYPE</td><td>Tipo de almacenamiento para archivos subidos. por defecto es <code>local</code></td><td>Enum String: <code>s3</code>, <code>local</code></td><td><code>local</code></td></tr><tr><td>BLOB_STORAGE_PATH</td><td>Ruta de carpeta local donde se almacenan los archivos subidos cuando <code>STORAGE_TYPE</code> es <code>local</code></td><td>String</td><td><code>your-home-dir/.flowise/storage</code></td></tr><tr><td>S3_STORAGE_BUCKET_NAME</td><td>Nombre del bucket para contener los archivos subidos cuando <code>STORAGE_TYPE</code> es <code>s3</code></td><td>String</td><td></td></tr><tr><td>S3_STORAGE_ACCESS_KEY_ID</td><td>AWS Access Key</td><td>String</td><td></td></tr><tr><td>S3_STORAGE_SECRET_ACCESS_KEY</td><td>AWS Secret Key</td><td>String</td><td></td></tr><tr><td>S3_STORAGE_REGION</td><td>Region para el bucket S3</td><td>String</td><td></td></tr></tbody></table>

## Ejemplos de cómo establecer environment variables:

### NPM

Puedes establecer todas estas variables al ejecutar Flowise usando npx. Por ejemplo:

```
npx flowise start --PORT=3000 --DEBUG=true
```

### Docker

```
docker run -d -p 5678:5678 flowise \
 -e DATABASE_TYPE=postgresdb \
 -e DATABASE_PORT=<POSTGRES_PORT> \
 -e DATABASE_HOST=<POSTGRES_HOST> \
 -e DATABASE_NAME=<POSTGRES_DATABASE_NAME> \
 -e DATABASE_USER=<POSTGRES_USER> \
 -e DATABASE_PASSWORD=<POSTGRES_PASSWORD> \
```

### Docker Compose

Puedes establecer todas estas variables en el archivo `.env` dentro de la carpeta `docker`. Consulta el archivo [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example).
