---
description: Learn how to use variables in Flowise
---

# Variables

***

Flowise allow users to create variables that can be used in the nodes. Variables can be Static or Runtime.

### Static

Static variable will be saved with the value specified, and retrieved as it is.

<figure><img src="../.gitbook/assets/image (13) (1) (1).png" alt="" width="542"><figcaption></figcaption></figure>

### Runtime

Value of the variable will be fetched from **.env** file using `process.env`

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="537"><figcaption></figcaption></figure>

### Override or setting variable through API

If there is an existing variable created, variable value provided in the API will override the existing value.

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

### Using Variables

Variables can be used by the nodes in Flowise. For instance, a variable named **`character`** is created:

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

We can then use this variable as **`$vars.<variable-name>`** in the Function of the following nodes:

* [Custom Tool](../integrations/langchain/tools/custom-tool.md)
* [Custom Function](../integrations/utilities/custom-js-function.md)
* [Custom Loader](../integrations/langchain/document-loaders/custom-document-loader.md)
* [If Else](../integrations/utilities/if-else.md)

<figure><img src="../.gitbook/assets/image (105).png" alt="" width="283"><figcaption></figcaption></figure>

Besides, user can also use the variable in text input of any node with the following format:

**`{{$vars.<variable-name>}}`**

For example, in Agent System Message:

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2) (1).png" alt="" width="508"><figcaption></figcaption></figure>

In Prompt Template:

<figure><img src="../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

## Resources

* [Pass Variables to Function](../integrations/langchain/tools/custom-tool.md#pass-variables-to-function)
