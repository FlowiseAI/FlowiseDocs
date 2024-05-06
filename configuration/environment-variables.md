# Environment Variables

Flowise supports different environment variables to configure your instance. You can specify the following variables in the `.env` file inside the `packages/server` folder. Refer to the [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/.env.example) file.

<table><thead><tr><th width="222">Variable</th><th>Description</th><th width="151">Type</th><th>Default</th></tr></thead><tbody><tr><td>PORT</td><td>The HTTP port Flowise runs on</td><td>Number</td><td>3000</td></tr><tr><td>FLOWISE_USERNAME</td><td>Username to login</td><td>String</td><td></td></tr><tr><td>FLOWISE_PASSWORD</td><td>Password to login</td><td>String</td><td></td></tr><tr><td>APIKEY_PATH</td><td>Location where API keys are saved</td><td>String</td><td><code>Flowise/packages/server</code></td></tr><tr><td>NUMBER_OF_PROXIES</td><td>Rate Limit Proxy</td><td>Number</td><td></td></tr><tr><td>CORS_ORIGINS</td><td>The allowed origins for all cross-origin HTTP calls </td><td>String</td><td></td></tr><tr><td>IFRAME_ORIGINS</td><td>The allowed origins for iframe src embedding</td><td>String</td><td></td></tr></tbody></table>

## Database

| Variable | Description | Type | Default |
| --- | --- | --- | --- |
| DATABASE_TYPE | Type of database to store the flowise data | Enum String: `sqlite`, `mysql`, `postgres` | `sqlite` |
| DATABASE_PATH | Location where database is saved (When DATABASE_TYPE is sqlite) | String | `your-home-dir/.flowise` |
| DATABASE_HOST | Host URL or IP address (When DATABASE_TYPE is not sqlite) | String |  |
| DATABASE_PORT | Database port (When DATABASE_TYPE is not sqlite) | String |  |
| DATABASE_USER | Database username (When DATABASE_TYPE is not sqlite) | String |  |
| DATABASE_PASSWORD | Database password (When DATABASE_TYPE is not sqlite) | String |  |
| DATABASE_NAME | Database name (When DATABASE_TYPE is not sqlite) | String |  |
| DATABASE_SSL | Database SSL is required (When DATABASE_TYPE is not sqlite) | Boolean: `true` or `false` | `false` |

## LangSmith Tracing

Flowise supports [LangSmith](https://docs.smith.langchain.com/) tracing with the following env variables:

<table><thead><tr><th width="247">Variable</th><th>Description</th><th width="151">Type</th></tr></thead><tbody><tr><td>LANGCHAIN_TRACING_V2</td><td>Turn LangSmith tracing ON or OFF</td><td>Enum String: <code>true</code>, <code>false</code></td></tr><tr><td>LANGCHAIN_ENDPOINT</td><td>LangSmith endpoint</td><td>String</td></tr><tr><td>LANGCHAIN_API_KEY</td><td>LangSmith API Key</td><td>String</td></tr><tr><td>LANGCHAIN_PROJECT</td><td>Project to trace on LangSmith</td><td>String</td></tr></tbody></table>

Watch how connect Flowise and LangSmith

{% embed url="https://youtu.be/BGGhRxS3AVQ" %}

## Built-In and External Dependencies

For security reasons, by default Tool Function only allows certain dependencies. It's possible to lift that restriction for built-in and external modules by setting the following environment variables:

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

## Debug and Logs

| Variable   | Description                         | Type                                             |                                |
| ---------- | ----------------------------------- | ------------------------------------------------ | ------------------------------ |
| DEBUG      | Print logs from components          | Boolean                                          |                                |
| LOG\_PATH  | Location where log files are stored | String                                           | `Flowise/packages/server/logs` |
| LOG\_LEVEL | Different levels of logs            | Enum String: `error`, `info`, `verbose`, `debug` | `info`                         |

`DEBUG`: if set to true, will print logs to terminal/console:

<figure><img src="../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

`LOG_LEVEL`: Different log levels for loggers to be saved. Can be `error`, `info`, `verbose`, or `debug.` By default it is set to `info,` only `logger.info` will be saved to the log files. If you want to have complete details, set to `debug`.

<figure><img src="../.gitbook/assets/image (2) (4).png" alt=""><figcaption><p><strong>server-requests.log.jsonl - logs every request sent to Flowise</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><strong>server.log - logs general actions on Flowise</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (4).png" alt=""><figcaption><p><strong>server-error.log - logs error with stack trace</strong></p></figcaption></figure>

## Credential

Flowise stores your third party API keys as encrypted credentials using an encryption key.

By default, a random encryption key will be generated when starting up the application and stored under a file path. This encryption key is then retrieved every time to decrypt the credentials used within a chatflow. For example, your OpenAI API key, Pinecone API key, etc.

| Variable                      | Description                                                                  | Type   |                           |
| ----------------------------- | ---------------------------------------------------------------------------- | ------ | ------------------------- |
| SECRETKEY\_PATH               | Location where encryption key (used to encrypt/decrypt credentials) is saved | String | `Flowise/packages/server` |
| FLOWISE\_SECRETKEY\_OVERWRITE | Encryption key to be used instead of the key stored in SECRETKEY\_PATH       | String |                           |

For some reasons, sometimes an encryption key might be re-generated or the stored path is changed, this will cause errors like - <mark style="color:red;">Credentials could not be decrypted.</mark>&#x20;

To avoid this, you can set your own encryption key as `FLOWISE_SECRETKEY_OVERWRITE`, so that the same encryption key will be used every time. There is no restriction on the format, you can set it as any text that you want, or the same as your `FLOWISE_PASSWORD`.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Credential API Key returned from the UI is not the same length as your original API Key that you have set. This is a fake prefix string that prevents network spoofing, that's why we are not returning the API Key back to the UI. However, the correct API Key will be retrieved and used during your interaction with the chatflow.
{% endhint %}

## Models

In some cases, you might want to use custom models on the existing Chat Model and LLM nodes, or restrict access to only certain models.

By default, Flowise pulls the model list from [here](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/models.json). However, users can create their own `models.json` file and specify the file path:

<table><thead><tr><th width="164">Variable</th><th width="196">Description</th><th width="78">Type</th><th>Default</th></tr></thead><tbody><tr><td>MODEL_LIST_CONFIG_JSON</td><td>Link to load list of models from your <code>models.json</code> config file</td><td>String</td><td><a href="https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json">https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json</a></td></tr></tbody></table>

## Storage

Flowise store the following files under a local path folder by default.

* Files uploaded on [Document Loaders](../integrations/langchain/document-loaders/)
* Image/Audio uploads from chat
* Images/Files from Assistant

User can specify `STORAGE_TYPE` to use AWS S3 or local path

<table><thead><tr><th width="227">Variable</th><th width="196">Description</th><th width="131">Type</th><th>Default</th></tr></thead><tbody><tr><td>STORAGE_TYPE</td><td>Type of storage for uploaded files. default is <code>local</code></td><td>Enum String: <code>s3</code>, <code>local</code></td><td><code>local</code></td></tr><tr><td>BLOB_STORAGE_PATH</td><td>Local folder path where uploaded files are stored when <code>STORAGE_TYPE</code> is <code>local</code></td><td>String</td><td><code>your-home-dir/.flowise/storage</code></td></tr><tr><td>S3_STORAGE_BUCKET_NAME</td><td>Bucket name to hold the uploaded files when <code>STORAGE_TYPE</code> is <code>s3</code></td><td>String</td><td></td></tr><tr><td>S3_STORAGE_ACCESS_KEY_ID</td><td>AWS Access Key</td><td>String</td><td></td></tr><tr><td>S3_STORAGE_SECRET_ACCESS_KEY</td><td>AWS Secret Key</td><td>String</td><td></td></tr><tr><td>S3_STORAGE_REGION</td><td>Region for S3 bucket</td><td>String</td><td></td></tr></tbody></table>

## How to set environment variables

### NPM

You can set all these variables when running Flowise using npx. For example:

```
npx flowise start --PORT=3000 --DEBUG=true
```

### Docker

You can set all these variables in the `.env` file inside the `docker` folder. Refer to the [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example) file.

### Render

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

### Railway

<figure><img src="../.gitbook/assets/image (9) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
