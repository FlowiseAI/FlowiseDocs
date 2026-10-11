---
description: 了解如何在 Flowise 中使用变量
---

# 变量

***

Flowise 允许用户创建可在节点中使用的变量。变量可以是静态的或运行时的。

### 静态

静态变量将使用指定的值保存，并按原样检索。

<figure><img src="../.gitbook/assets/image (13) (1) (1) (1) (1) (1).png" alt="" width="542"><figcaption></figcaption></figure>

### 运行时

变量的值将使用 `process.env` 从 **.env** 文件中获取

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="537"><figcaption></figcaption></figure>

### 通过 API 覆盖或设置变量

为了覆盖变量值，用户必须从右上角的按钮显式启用它：

**设置** -> **配置** -> **安全** 选项卡：

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

如果创建了现有变量，API 中提供的变量值将覆盖现有值。

```json
{
    "question": "hello",
    "overrideConfig": {
        "vars": {
            "var": "some-override-value"
        }
    }
}
```

### 使用变量

Flowise 中的节点可以使用变量。例如，创建一个名为 **`character`** 的变量：

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

然后我们可以使用这个变量作为 **`$vars。<variable-name>`** 在以下节点的 Function 中：

* [自定义工具](../integrations/langchain/tools/custom-tool.md)
* [自定义函数](../integrations/utilities/custom-js-function.md)
* [自定义加载程序](../integrations/langchain/document-loaders/custom-document-loader.md)
* [如果否则](../integrations/utilities/if-else.md)
* 自定义 MCP

<figure><img src="../.gitbook/assets/image (105).png" alt="" width="283"><figcaption></figcaption></figure>

此外，用户还可以在任意节点的文本输入中使用该变量，格式如下：

**`{{$vars。<variable-name>}}`**

例如，在代理系统消息中：

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2) (1).png" alt="" width="508"><figcaption></figcaption></figure>

在提示词模板中：

<figure><img src="../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

## 资源

* [将变量传递给函数](../integrations/langchain/tools/custom-tool.md#pass-variables-to-function)
