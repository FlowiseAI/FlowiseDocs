# Environment Variables

Flowise support different environment variables to configure your instance. You can specify the following variables in the `.env` file inside `packages/server` folder. Refer to [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/.env.example) file.

<table><thead><tr><th width="222">Variable</th><th>Description</th><th width="151">Type</th><th>Default</th></tr></thead><tbody><tr><td>PORT</td><td>The HTTP port Flowise runs on</td><td>Number</td><td>3000</td></tr><tr><td>FLOWISE_USERNAME</td><td>Username to login</td><td>String</td><td></td></tr><tr><td>FLOWISE_PASSWORD</td><td>Password to login</td><td>String</td><td></td></tr><tr><td>DEBUG</td><td>Print logs onto terminal/console</td><td>Boolean</td><td></td></tr><tr><td>APIKEY_PATH</td><td>Location where API keys are saved</td><td>String</td><td><code>Flowise/packages/server</code></td></tr><tr><td>SECRETKEY_PATH</td><td>Location where encryption key (used to encrypt/decrypt credentials) is saved</td><td>String</td><td><code>Flowise/packages/server</code></td></tr><tr><td>FLOWISE_SECRETKEY_OVERWRITE</td><td>Encryption key to be used instead of the key stored in SECRETKEY_PATH</td><td>String</td><td></td></tr><tr><td>LOG_PATH</td><td>Location where log files are stored</td><td>String</td><td><code>Flowise/logs</code></td></tr><tr><td>LOG_LEVEL</td><td>Different log levels for loggers to be saved</td><td>Enum String: <code>info</code>, <code>verbose</code>, <code>debug</code></td><td><code>info</code></td></tr><tr><td>TOOL_FUNCTION_BUILTIN_DEP</td><td>NodeJS built-in modules to be used for Tool Function</td><td>String</td><td></td></tr><tr><td>TOOL_FUNCTION_EXTERNAL_DEP</td><td>External modules to be used for Tool Function</td><td>String</td><td></td></tr><tr><td>NUMBER_OF_PROXIES</td><td>Rate Limit Proxy</td><td>Number</td><td></td></tr></tbody></table>

## Database Env Variables

<table><thead><tr><th width="222">Variable</th><th>Description</th><th width="151">Type</th><th>Default</th></tr></thead><tbody><tr><td>DATABASE_TYPE</td><td>Type of database to store the flowise data</td><td>Enum String: <code>sqlite</code>, <code>mysql</code>, <code>postgres</code></td><td><code>sqlite</code></td></tr><tr><td>DATABASE_PATH</td><td>Location where database is saved (When DATABASE_TYPE is sqlite)</td><td>String</td><td><code>your-home-dir/.flowise</code></td></tr><tr><td>DATABASE_HOST</td><td>Host URL or IP address (When DATABASE_TYPE is not sqlite)</td><td>String</td><td></td></tr><tr><td>DATABASE_PORT</td><td>Database port (When DATABASE_TYPE is not sqlite)</td><td>String</td><td></td></tr><tr><td>DATABASE_USER</td><td>Database username (When DATABASE_TYPE is not sqlite)</td><td>String</td><td></td></tr><tr><td>DATABASE_PASSWORD</td><td>Database password (When DATABASE_TYPE is not sqlite)</td><td>String</td><td></td></tr><tr><td>DATABASE_NAME</td><td>Database name (When DATABASE_TYPE is not sqlite)</td><td>String</td><td></td></tr></tbody></table>

## LangSmith Tracing

Flowise supports [LangSmith](https://docs.smith.langchain.com/) tracing with the following env variables:

<table><thead><tr><th width="247">Variable</th><th>Description</th><th width="151">Type</th></tr></thead><tbody><tr><td>LANGCHAIN_TRACING_V2</td><td>Turn LangSmith tracing ON or OFF</td><td>Enum String: <code>true</code>, <code>false</code></td></tr><tr><td>LANGCHAIN_ENDPOINT</td><td>LangSmith endpoint</td><td>String</td></tr><tr><td>LANGCHAIN_API_KEY</td><td>LangSmith API Key</td><td>String</td></tr><tr><td>LANGCHAIN_PROJECT</td><td>Project to trace on LangSmith</td><td>String</td></tr></tbody></table>

Watch how connect Flowise and LangSmith

{% embed url="https://youtu.be/BGGhRxS3AVQ" %}

## Built-In and External Dependencies

For security reasons, by default Tool Function only allow certain dependencies. It's possible to lift that restriction for built-in and external modules by setting the following environment variables:

* `TOOL_FUNCTION_BUILTIN_DEP`: For built-in modules
* `TOOL_FUNCTION_EXTERNAL_DEP`: For external modules sourced from Flowise/node\_modules directory

```bash
# Allows usage of all builtin modules
export TOOL_FUNCTION_BUILTIN_DEP=*

# Allows usage of only fs
export TOOL_FUNCTION_BUILTIN_DEP=fs

# Allows usage of only crypto and fs
export TOOL_FUNCTION_BUILTIN_DEP=crypto,fs

# Allow usage of external npm modules.
export TOOL_FUNCTION_EXTERNAL_DEP=axios,moment
```

## Debug and Logs

* `DEBUG`: if set to true, will print logs to terminal/console:

<figure><img src="../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

* `LOG_LEVEL`: Different log levels for loggers to be saved. Can be `error`, `info`, `verbose`, or `debug.` By default it is set to `info,` only `logger.info` will be saved to the log files. If you want to have complete details, set to `debug`.

<figure><img src="../.gitbook/assets/image (2) (4).png" alt=""><figcaption><p><strong>server-requests.log.jsonl - logs every request sent to Flowise</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><strong>server.log - logs general actions on Flowise</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (4).png" alt=""><figcaption><p><strong>server-error.log - logs error with stack trace</strong></p></figcaption></figure>

## Credential

Flowise store your third party API keys as encrypted credentials using an encryption key.

By default, a random encryption key will be generated when starting up the application and stored under a file path. This encryption key is then retrieved everytime to decrypt the credentials used within a chatflow. For example, your OpenAI API key, Pinecone API key, etc.

* `SECRETKEY_PATH`: Where the encryption key is being stored
* `FLOWISE_SECRETKEY_OVERWRITE`: Overwrite the encryption key stored in `SECRETKEY_PATH`

For some reasons, sometimes encryption key might be re-generated or the stored path was changed, this will cause errors like - <mark style="color:red;">Credentials could not be decrypted.</mark> To avoid this, you can set your own encryption key as `FLOWISE_SECRETKEY_OVERWRITE`, so that the same encryption key will be used everytime. There is no restriction on the format, you can set it as any text that you want, or the same as your `FLOWISE_PASSWORD`.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Credential API Key returned from the UI is not the same length as your original Api Key that you have set. This is a fake prefix string that prevents network spoofing, that's why we are not returning the Api Key back to UI. However, the correct Api Key will be retrieved and used during your interaction with the chatflow.
{% endhint %}

## NPM

You can set all these variables when running Flowise using npx. For example:

```
npx flowise start --PORT=3000 --DEBUG=true
```

## Docker

You can set all these variables in the `.env` file inside `docker` folder. Refer to [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example) file.

## Render

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

## Railway

<figure><img src="../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption></figcaption></figure>
