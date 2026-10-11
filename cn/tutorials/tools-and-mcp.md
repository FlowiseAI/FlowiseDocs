# 工具 & MCP

在之前的[**与 API**](interacting-with-api.md) 交互教程中，我们探讨了如何使 LLM 能够调用外部 API。为了增强用户体验，Flowise 提供了一系列预构建工具。请参阅[**工具**](../integrations/langchain/tools/) 部分，了解可用集成的完整列表。

如果您需要的工具尚不可用，您可以创建一个**自定义工具**来满足您的要求。

## 自定义工具

我们将使用相同的[事件管理服务器](interacting-with-api.md#prerequisite)，并创建一个自定义工具，该工具可以调用 HTTP POST 请求 `/events`。

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

* **工具名称：** `create_event`
* **工具说明：** `Use this when you want to create a new event.`
* **输入架构：** API 请求正文的 JSON 架构，允许 LLM 了解如何自动生成正确的 JSON 正文。例如：
* **Javascript函数**：调用此工具后要执行的实际函数

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

### 如何使用函数：

* 您可以使用 Flowise 中导入的任何库。
* 您可以使用输入架构中指定的属性作为前缀为 `$` 的变量：
  * 输入架构的属性 = `name`
  * 函数中使用的变量 = `$name`
* 您可以获得默认流配置：
  * `$flow.sessionId`
  * `$flow.chatId`
  * `$flow.chatflowId`
  * `$flow.input`
  * `$flow.state`
* 您可以获取自定义变量：`$vars.<variable-name>`
* 函数末尾必须返回一个字符串值

### 在 Agent 上使用自定义工具

创建自定义工具后，您可以在Agent节点上使用它。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="341"><figcaption></figcaption></figure>

从工具下拉列表中，选择自定义工具。如果您想直接返回自定义工具的输出，您还可以打开**Return Direc**t。

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="392"><figcaption></figcaption></figure>

### 在 工具 上使用自定义工具

它还可以在确定的工作流程场景中用作工具节点。\
在这种情况下，**工具输入参数必须显式定义并填充值**，因为没有 LLM 来自动确定值。

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## MCP

MCP（[模型上下文协议](https://modelcontextprotocol.io/introduction)）提供了一种将 AI 模型连接到不同数据源和工具的标准化方法。换句话说，人们可以使用其他人创建的 MCP 服务器，而不是依赖 Flowise 内置工具或创建自定义工具。 MCP 被广泛认为是行业标准，通常由官方提供商支持和维护。例如，GitHub MCP 由 GitHub 团队开发和维护，并为 Atlassian Jira、Brave Search 等提供类似的支持。您可以在[此处](https://modelcontextprotocol.io/examples)找到支持的服务器列表。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="413"><figcaption></figcaption></figure>

## 自定义 MCP

除了预构建的 MCP 工具外，最强大的功能是 **自定义 MCP**，它允许用户连接到他们选择的任何 MCP 服务器。

MCP 遵循客户端-服务器架构，其中：

* **主机**是发起连接的 LLM 应用程序（如 Flowise）
* **客户端** 在主机应用程序内部与服务器保持 1:1 连接（如自定义 MCP）
* **服务器** 向客户端提供上下文、工具和提示词（例如 [servers](https://modelcontextprotocol.io/examples)）

处理客户端和服务器之间的实际通信。 MCP 支持多种传输机制：

1. **Stdio 传输**
* 使用标准输入/输出进行通信
   * 本地流程的理想选择
2. **可流式 HTTP 传输**
   * 使用 HTTP 和可选的服务器发送事件进行流式传输
   * HTTP POST 用于客户端到服务器的消息

### 工作室

Stdio 传输支持通过标准输入和输出流进行通信。这对于本地集成和命令行工具特别有用。

仅在本地使用 Flowise 时使用此选项，而不是在部署到云服务时使用此选项。这是因为运行像 `npx` 这样的命令将在本地安装 MCP 服务器包（例如：`@modelcontextprotocol/server-sequential-thinking`），并且通常需要很长时间。 

它更适合桌面应用程序，如 Claude Desktop、VS Code 等。

#### **NPX 命令**

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

对于 Windows，请参阅此[指南](https://gist.github.com/feveromo/7a340d7795fca1ccd535a5802b976e1f)。

#### **Docker 命令**

当运行 Flowise 的机器也可以访问 Docker 时，Docker 命令适用。但是，它不适合部署在 Docker 访问受限或不可用的云服务上。

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

Docker 提供了 MCP 服务器列表，可在[此处](https://hub.docker.com/catalogs/mcp) 找到。它的工作原理如下：

1. 确保 Docker 正在运行。
2. 找到 MCP 服务器配置并将其添加到 **自定义 MCP**。例如：[https://hub.docker.com/r/mcp/sequentialthinking](https://hub.docker.com/r/mcp/sequentialthinking)
3. 刷新**可用操作**。如果本地没有找到镜像，Docker会自动拉取最新的镜像。拉取图像后，您将看到可用操作的列表。

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

#### 何时使用

* 构建命令行工具
* 实施本地集成
* 需要简单的进程通信
* 使用 shell 脚本

### 可流式传输 HTTP （推荐）

我们将使用 Github Remote MCP 作为示例。 [远程 GitHub MCP 服务器](https://github.com/github/github-mcp-server) 的美妙部分，您无需在本地安装或运行它，新更新会自动应用。

#### 步骤 1：为 Github 创建变量 PAT

为了访问 MCP 服务器，我们需要从 Github 创建个人访问令牌。请参阅[指南](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic)。创建 PAT 后，创建一个变量来存储令牌。该变量将在自定义 MCP 中使用。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="508"><figcaption></figcaption></figure>

#### 步骤 2：创建自定义 MCP

创建一个代理节点，并添加一个新的自定义 MCP 工具。对于可流式传输的 HTTP，我们只需放入 URL 和其他必要的标头。您可以在 MCP 服务器配置中使用 [variables](../using-flowise/variables.md)，并带有双大括号 `{{ }}` 和前缀 `$vars。<variableName>`。

```json
{
  "url": "https://api.githubcopilot.com/mcp/",
  "headers": {
    "Authorization": "Bearer {{$vars.githubPAT}}",
  }
}
```

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="414"><figcaption></figcaption></figure>

#### 第 3 步：选择操作

如果 MCP 服务器配置正常工作，您可以刷新 **可用操作**，Flowise 将自动从 MCP 服务器提取所有可用操作。

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt="" width="359"><figcaption></figcaption></figure>

#### 交互示例：

> 给我最新一期

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

代理能够从 MCP 识别适当的操作，并使用它们来回答用户的查询。

#### 何时使用

在以下情况下使用 Streamable HTTP：

* 构建基于网络的集成
* 需要通过 HTTP 进行客户端-服务器通信
* 需要有状态会话
* 支持多个并发客户端
* 实现可恢复连接

## 视频教程

{% embed url="https://youtu.be/7FClI-QM3tk?si=zBNEShd3NlcrOBrO" %}
