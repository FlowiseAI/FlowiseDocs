# Environment Variables

Flowise support different environment variables to configure your instance. You can specify the following variables in the `.env` file inside `packages/server` folder. Refer to [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/.env.example) file.

<table><thead><tr><th width="222">Variable</th><th>Description</th><th width="151">Type</th><th>Default</th></tr></thead><tbody><tr><td>PORT</td><td>The HTTP port Flowise runs on</td><td>Number</td><td>3000</td></tr><tr><td>FLOWISE_USERNAME</td><td>Username to login</td><td>String</td><td></td></tr><tr><td>FLOWISE_PASSWORD</td><td>Password to login</td><td>String</td><td></td></tr><tr><td>DEBUG</td><td>Show logs from every calls</td><td>Boolean</td><td></td></tr><tr><td>DATABASE_PATH</td><td>Location where database is saved</td><td>String</td><td><code>your-home-dir/.flowise</code></td></tr><tr><td>APIKEY_PATH</td><td>Location where api keys are saved</td><td>String</td><td><code>Flowise/packages/server</code></td></tr><tr><td>SECRETKEY_PATH</td><td>Location where encryption key (used to encrypt/decrypt credentials) is saved</td><td>String</td><td><code>Flowise/packages/server</code></td></tr><tr><td>LOG_PATH</td><td>Location where log files are stored</td><td>String</td><td><code>Flowise/logs</code></td></tr><tr><td>LOG_LEVEL</td><td>Set different levels of logs</td><td>Enum String: <code>info</code>, <code>verbose</code>, <code>debug</code></td><td><code>info</code></td></tr><tr><td>EXECUTION_MODE</td><td>Whether predictions run in their own process or the main process</td><td>Enum String: <code>child,</code> <code>main</code></td><td><code>main</code></td></tr><tr><td>TOOL_FUNCTION_BUILTIN_DEP</td><td>NodeJS built-in modules to be used for Tool Function</td><td>String</td><td></td></tr><tr><td>TOOL_FUNCTION_EXTERNAL_DEP</td><td>External modules to be used for Tool Function</td><td>String</td><td></td></tr></tbody></table>

## Execution Mode

When EXECUTION\_MODE is set to `child` , each prediction will spawn a new [NodeJS child process](https://nodejs.org/api/child\_process.html). These child processes can run simultaneously, proving a multicore functionality. However, more resources (CPU/RAM) are needed for the additional processes.

When EXECUTION\_MODE is set to `main`, each prediction will run in the main thread. The drawback is that it can't take advantage of multiple CPUs.

We recommend using `main` mode in general unless you have multiple intensive prediction tasks that have to be carried out simultaneously.

## BuiltIn and External Dependencies

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

## NPM

You can set all these variables when running Flowise using npx. For example:

```
npx flowise start --PORT=3000 --DEBUG=true
```

## Docker

You can set all these variables in the `.env` file inside `docker` folder. Refer to [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example) file.

## Render

<figure><img src=".gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

## Railway

<figure><img src=".gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>
