---
description: Learn how to configure environment variables for Flowise
---

# Environment Variables

Flowise support different environment variables to configure your instance. You can specify the following variables in the `.env` file inside `packages/server` folder. Refer to [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/.env.example) file.

<table><thead><tr><th width="233">Variable</th><th width="219">Description</th><th width="104">Type</th><th>Default</th></tr></thead><tbody><tr><td>PORT</td><td>The HTTP port Flowise runs on</td><td>Number</td><td>3000</td></tr><tr><td>FLOWISE_FILE_SIZE_LIMIT</td><td>Maximum file size when uploading</td><td>String</td><td><code>50mb</code></td></tr><tr><td>NUMBER_OF_PROXIES</td><td>Rate Limit Proxy</td><td>Number</td><td></td></tr><tr><td>CORS_ORIGINS</td><td>The allowed origins for all cross-origin HTTP calls</td><td>String</td><td></td></tr><tr><td>IFRAME_ORIGINS</td><td>The allowed origins for iframe src embedding</td><td>String</td><td></td></tr><tr><td>SHOW_COMMUNITY_NODES</td><td>Display nodes that are created by community</td><td>Boolean: <code>true</code> or <code>false</code></td><td></td></tr><tr><td>DISABLED_NODES</td><td>Comma separated list of node names to disable</td><td>String</td><td></td></tr></tbody></table>

## For Database

| Variable           | Description                                                      | Type                                       | Default                  |
| ------------------ | ---------------------------------------------------------------- | ------------------------------------------ | ------------------------ |
| DATABASE\_TYPE     | Type of database to store the flowise data                       | Enum String: `sqlite`, `mysql`, `postgres` | `sqlite`                 |
| DATABASE\_PATH     | Location where database is saved (When DATABASE\_TYPE is sqlite) | String                                     | `your-home-dir/.flowise` |
| DATABASE\_HOST     | Host URL or IP address (When DATABASE\_TYPE is not sqlite)       | String                                     |                          |
| DATABASE\_PORT     | Database port (When DATABASE\_TYPE is not sqlite)                | String                                     |                          |
| DATABASE\_USER     | Database username (When DATABASE\_TYPE is not sqlite)            | String                                     |                          |
| DATABASE\_PASSWORD | Database password (When DATABASE\_TYPE is not sqlite)            | String                                     |                          |
| DATABASE\_NAME     | Database name (When DATABASE\_TYPE is not sqlite)                | String                                     |                          |
| DATABASE\_SSL      | Database SSL is required (When DATABASE\_TYPE is not sqlite)     | Boolean: `true` or `false`                 | `false`                  |

## For Storage

Flowise store the following files under a local path folder by default.

* Files uploaded on [Document Loaders](../integrations/langchain/document-loaders/)/Document Store
* Image/Audio uploads from chat
* Images/Files from Assistant
* Files from [Vector Upsert API](../using-flowise/api.md#vector-upsert-api)

User can specify `STORAGE_TYPE` to use AWS S3, Google Cloud Storage or local path

| Variable                               | Description                                                                      | Type                              | Default                          |
| -------------------------------------- | -------------------------------------------------------------------------------- | --------------------------------- | -------------------------------- |
| STORAGE\_TYPE                          | Type of storage for uploaded files. default is `local`                           | Enum String: `s3`, `gcs`, `local` | `local`                          |
| BLOB\_STORAGE\_PATH                    | Local folder path where uploaded files are stored when `STORAGE_TYPE` is `local` | String                            | `your-home-dir/.flowise/storage` |
| S3\_STORAGE\_BUCKET\_NAME              | Bucket name to hold the uploaded files when `STORAGE_TYPE` is `s3`               | String                            |                                  |
| S3\_STORAGE\_ACCESS\_KEY\_ID           | AWS Access Key                                                                   | String                            |                                  |
| S3\_STORAGE\_SECRET\_ACCESS\_KEY       | AWS Secret Key                                                                   | String                            |                                  |
| S3\_STORAGE\_REGION                    | Region for S3 bucket                                                             | String                            |                                  |
| S3\_ENDPOINT\_URL                      | Custom S3 endpoint (optional)                                                    | String                            |                                  |
| S3\_FORCE\_PATH\_STYLE                 | Force S3 path style (optional)                                                   | Boolean                           | false                            |
| GOOGLE\_CLOUD\_STORAGE\_CREDENTIAL     | Google Cloud Service Account Key                                                 | String                            |                                  |
| GOOGLE\_CLOUD\_STORAGE\_PROJ\_ID       | Google Cloud Project ID                                                          | String                            |                                  |
| GOOGLE\_CLOUD\_STORAGE\_BUCKET\_NAME   | Google Cloud Storage Bucket Name                                                 | String                            |                                  |
| GOOGLE\_CLOUD\_UNIFORM\_BUCKET\_ACCESS | Type of Access                                                                   | Boolean                           | true                             |

## For Debugging and Logs

| Variable   | Description                         | Type                                             |                                |
| ---------- | ----------------------------------- | ------------------------------------------------ | ------------------------------ |
| DEBUG      | Print logs from components          | Boolean                                          |                                |
| LOG\_PATH  | Location where log files are stored | String                                           | `Flowise/packages/server/logs` |
| LOG\_LEVEL | Different levels of logs            | Enum String: `error`, `info`, `verbose`, `debug` | `info`                         |

`DEBUG`: if set to true, will print logs to terminal/console:

<figure><img src="../.gitbook/assets/image (3) (3) (1).png" alt=""><figcaption></figcaption></figure>

`LOG_LEVEL`: Different log levels for loggers to be saved. Can be `error`, `info`, `verbose`, or `debug.` By default it is set to `info,` only `logger.info` will be saved to the log files. If you want to have complete details, set to `debug`.

<figure><img src="../.gitbook/assets/image (2) (4).png" alt=""><figcaption><p><strong>server-requests.log.jsonl - logs every request sent to Flowise</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><strong>server.log - logs general actions on Flowise</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (4).png" alt=""><figcaption><p><strong>server-error.log - logs error with stack trace</strong></p></figcaption></figure>

### Logs Streaming S3

When `STORAGE_TYPE` env variable is set to `s3` , logs will be automatically streamed and stored to S3. New log file will be created hourly, enabling easier debugging.

### Logs Streaming GCS

When `STORAGE_TYPE` env variable is set to `gcs` , logs will be automatically streamed to Google [Cloud Logging](https://cloud.google.com/logging?hl=en).

## For Credentials

Flowise store your third party API keys as encrypted credentials using an encryption key.

By default, a random encryption key will be generated when starting up the application and stored under a file path. This encryption key is then retrieved everytime to decrypt the credentials used within a chatflow. For example, your OpenAI API key, Pinecone API key, etc.

You can configure to use AWS Secret Manager to store the encryption key instead.

| Variable                      | Description                                           | Type                        | Default                   |
| ----------------------------- | ----------------------------------------------------- | --------------------------- | ------------------------- |
| SECRETKEY\_STORAGE\_TYPE      | How to store the encryption key                       | Enum String: `local`, `aws` | `local`                   |
| SECRETKEY\_PATH               | Local file path where encryption key is saved         | String                      | `Flowise/packages/server` |
| FLOWISE\_SECRETKEY\_OVERWRITE | Encryption key to be used instead of the existing key | String                      |                           |
| SECRETKEY\_AWS\_ACCESS\_KEY   |                                                       | String                      |                           |
| SECRETKEY\_AWS\_SECRET\_KEY   |                                                       | String                      |                           |
| SECRETKEY\_AWS\_REGION        |                                                       | String                      |                           |

For some reasons, sometimes encryption key might be re-generated or the stored path was changed, this will cause errors like - <mark style="color:red;">Credentials could not be decrypted.</mark>

To avoid this, you can set your own encryption key as `FLOWISE_SECRETKEY_OVERWRITE`, so that the same encryption key will be used everytime. There is no restriction on the format, you can set it as any text that you want, or the same as your `FLOWISE_PASSWORD`.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Credential API Key returned from the UI is not the same length as your original Api Key that you have set. This is a fake prefix string that prevents network spoofing, that's why we are not returning the Api Key back to UI. However, the correct Api Key will be retrieved and used during your interaction with the chatflow.
{% endhint %}

## For Models

In some cases, you might want to use custom model on the existing Chat Model and LLM nodes, or restrict access to only certain models.

By default, Flowise pulls the model list from [here](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/models.json). However user can create their own `models.json` file and specify the file path:

<table><thead><tr><th width="164">Variable</th><th width="196">Description</th><th width="78">Type</th><th>Default</th></tr></thead><tbody><tr><td>MODEL_LIST_CONFIG_JSON</td><td>Link to load list of models from your <code>models.json</code> config file</td><td>String</td><td><a href="https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json">https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json</a></td></tr></tbody></table>

## For API Keys (Deprecated)

Users can create multiple API keys within Flowise in order to authenticate with the [APIs](../using-flowise/api.md). By default, keys get stored as a JSON file to your local file path. User can change the behavior by using the below env variable.

| Variable              | Description                                                                                | Type                      | Default                   |
| --------------------- | ------------------------------------------------------------------------------------------ | ------------------------- | ------------------------- |
| APIKEY\_STORAGE\_TYPE | Method to store API keys                                                                   | Enum string: `json`, `db` | `json`                    |
| APIKEY\_PATH          | Location where the API keys are stored when `APIKEY_STORAGE_TYPE` is unspecified or `json` | String                    | `Flowise/packages/server` |

Using `db` as storage type will store the API keys to database instead of a local JSON file.

<figure><img src="../.gitbook/assets/image (254).png" alt=""><figcaption><p>Flowise API Keys</p></figcaption></figure>

## For Built-In and External Dependencies

There are certain nodes/features within Flowise that allow user to run Javascript code. For security reasons, by default it only allow certain dependencies. It's possible to lift that restriction for built-in and external modules by setting the following environment variables:

| Variable                      | Description                                          |        |
| ----------------------------- | ---------------------------------------------------- | ------ |
| TOOL\_FUNCTION\_BUILTIN\_DEP  | NodeJS built-in modules to be used for Tool Function | String |
| TOOL\_FUNCTION\_EXTERNAL\_DEP | External modules to be used for Tool Function        | String |

{% code title=".env" %}
```bash
# Allows usage of all builtin modules
TOOL_FUNCTION_BUILTIN_DEP=*

# Allows usage of only fs
TOOL_FUNCTION_BUILTIN_DEP=fs

# Allows usage of only crypto and fs
TOOL_FUNCTION_BUILTIN_DEP=crypto,fs

# Allow usage of external npm modules.
TOOL_FUNCTION_EXTERNAL_DEP=axios,moment
```
{% endcode %}

## Examples of how to set environment variables

### NPM

You can set all these variables when running Flowise using npx. For example:

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

You can set all these variables in the `.env` file inside `docker` folder. Refer to [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example) file.
