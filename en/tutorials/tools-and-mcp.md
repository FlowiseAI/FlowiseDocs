# Tools & MCP

In the previous [**Interacting with API**](interacting-with-api.md) tutorial, we explored how to enable LLMs to call external APIs. To enhance the user experience, Flowise provides a list of prebuilt tools. Refer to the [**Tools**](../integrations/langchain/tools/) section for the full list of available integrations.

In cases where the tool you need is not yet available, you can create a **Custom Tool** to suit your requirements.

## Custom Tool

We are going to use the same [Event Management Server](interacting-with-api.md#prerequisite), and create a custom tool which can call the HTTP POST request for `/events`.

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

* **Tool Name:** `create_event`
* **Tool Description:** `Use this when you want to create a new event.`
* **Input Schema:** A JSON schema of the API request body which allows LLM to know how to automatically generate the correct JSON body. For instance:
* **Javascript Function**: The actual function to execute once this tool is called

```javascript
const fetch = require('node-fetch');
const url = 'http://localhost:5566/events';
const options = {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      name: $name,
      location: $location,
      date: $date
    })
};
try {
    const response = await fetch(url, options);
    const text = await response.text();
    return text;
} catch (error) {
    console.error(error);
    return '';
}
```

### How to use function:

* You can use any libraries imported in Flowise.
* You can use properties specified in Input Schema as variables with prefix `$`:
  * Property from Input Schema = `name`
  * Variable to be used in Function = `$name`
* You can get default flow config:
  * `$flow.sessionId`
  * `$flow.chatId`
  * `$flow.chatflowId`
  * `$flow.input`
  * `$flow.state`
* You can get custom variables: `$vars.<variable-name>`
* Must return a string value at the end of function

### Use custom tool on Agent

After custom tool has been created, you can use it on the Agent node.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="341"><figcaption></figcaption></figure>

From the Tool dropdown, select the custom tool. You can also turn on **Return Direc**t if you want to directly return the output from custom tool.

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="392"><figcaption></figcaption></figure>

### Use custom tool on Tool

It can also be used as a Tool Node in a determined workflow scenario.\
In this case, **Tool Input Arguments must be explicitly defined and filled with values**, because there is no LLM to automatically determine the values.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## MCP

MCP ([Model Context Protocol](https://modelcontextprotocol.io/introduction)) provides a standardized way to connect AI models to different data sources and tools. In other words, instead of relying on Flowise built in tools or creating custom tool, one can uses MCP servers that have been created by others. MCP is widely considered an industry standard and is typically supported and maintained by the official providers. For example, the GitHub MCP is developed and maintained by the GitHub team, with similar support provided for Atlassian Jira, Brave Search, and others. You can find the list of supported servers [here](https://modelcontextprotocol.io/examples).

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="413"><figcaption></figcaption></figure>

## Custom MCP

Apart from the prebuilt MCP tools, the most powerful feature is **Custom MCP**, which allows users to connect to any MCP server of their choice.

MCP follows a client-server architecture where:

* **Hosts** are LLM applications (like Flowise) that initiate connections
* **Clients** maintain 1:1 connections with servers, inside the host application (like Custom MCP)
* **Servers** provide context, tools, and prompts to clients (example [servers](https://modelcontextprotocol.io/examples))

To handle the actual communication between clients and servers. MCP supports multiple transport mechanisms:

1. **Stdio transport**
   * Uses standard input/output for communication
   * Ideal for local processes
2. **Streamable HTTP transport**
   * Uses HTTP with optional Server-Sent Events for streaming
   * HTTP POST for client-to-server messages

### Stdio

Stdio transport enables communication through standard input and output streams. This is particularly useful for local integrations and command-line tools.

Only use this when using Flowise locally, not when deployed to cloud services. This is because running command like `npx` will install the MCP server package (ex: `@modelcontextprotocol/server-sequential-thinking`)  locally, and it often takes long time for that.&#x20;

It is more suited for desktop application like Claude Desktop, VS Code etc.

#### **NPX command**

```json
{
  "command": "npx",
  "args": [
    "-y",
    "@modelcontextprotocol/server-sequential-thinking"
  ]
}
```

<figure><img src="../.gitbook/assets/image (16) (1) (1).png" alt="" width="419"><figcaption></figcaption></figure>

For Windows, refer to this [guide](https://gist.github.com/feveromo/7a340d7795fca1ccd535a5802b976e1f).

#### **Docker command**

The Docker command is suitable when the machine running Flowise also has access to Docker. However, it is not suitable for deployments on cloud services where Docker access is restricted or unavailable.

```json
{
  "command": "docker",
  "args": [
    "run",
    "-i",
    "--rm",
    "mcp/sequentialthinking"
  ]
}
```

<figure><img src="../.gitbook/assets/image (312).png" alt="" width="416"><figcaption></figcaption></figure>

Docker provides a list of MCP servers, which can be found [here](https://hub.docker.com/catalogs/mcp). Here's how it works:

1. Make sure Docker is running.
2. Locate the MCP server configuration and add it to **Custom MCP**. For example: [https://hub.docker.com/r/mcp/sequentialthinking](https://hub.docker.com/r/mcp/sequentialthinking)
3. Refresh the **Available Actions**. If the image is not found locally, Docker will automatically pull the latest image. Once the image is pulled, you will see the list of available actions.

```
Unable to find image 'mcp/sequentialthinking:latest' locally
latest: Pulling from mcp/sequentialthinking
f18232174bc9: Already exists
cb2bde55f71f: Pull complete
9d0e0719fbe0: Pull complete
6f063dbd7a5d: Pull complete
93a0fbe48c24: Pull complete
e2e59f8d7891: Pull complete
96ec0bda7033: Pull complete
4f4fb700ef54: Pull complete
d0900e07408c: Pull complete
Digest: sha256:cd3174b2ecf37738654cf7671fb1b719a225c40a78274817da00c4241f465e5f
Status: Downloaded newer image for mcp/sequentialthinking:latest
Sequential Thinking MCP Server running on stdio
```

#### When to use

* Building command-line tools
* Implementing local integrations
* Needing simple process communication
* Working with shell scripts

### Streamable HTTP (Recommended)

We will use Github Remote MCP as an example. The beautiful part of [Remote GitHub MCP server](https://github.com/github/github-mcp-server), you don’t need to install or run it locally, new updates are applied automatically.

#### Step 1: Create a variable for Github PAT

In order to access the MCP server, we need to create a Personal Access Token from Github. Refer to [guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic). Once PAT has been created, create a variable to store the token. This variable will be used in Custom MCP.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="508"><figcaption></figcaption></figure>

#### Step 2: Create Custom MCP

Create an Agent node, and add a new Custom MCP tool. For streamable HTTP, we just need to put in the URL and other necessary headers. You can use [variables](../using-flowise/variables.md) in the MCP Server Config with double curly braces `{{ }}` and prefix `$vars.<variableName>`.

```json
{
  "url": "https://api.githubcopilot.com/mcp/",
  "headers": {
    "Authorization": "Bearer {{$vars.githubPAT}}",
  }
}
```

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="414"><figcaption></figcaption></figure>

#### Step 3: Select the actions

If the MCP server configuration is working correctly, you can refresh the **Available Actions**, and Flowise will automatically pull in all available actions from the MCP server.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt="" width="359"><figcaption></figcaption></figure>

#### Example Interactions:

> Give me the most recent issue

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The agent is able to identify the appropriate actions from MCP and use them to answer the user's query.

#### When to use

Use Streamable HTTP when:

* Building web-based integrations
* Needing client-server communication over HTTP
* Requiring stateful sessions
* Supporting multiple concurrent clients
* Implementing resumable connections

## Video Tutorial

{% embed url="https://youtu.be/7FClI-QM3tk?si=zBNEShd3NlcrOBrO" %}
