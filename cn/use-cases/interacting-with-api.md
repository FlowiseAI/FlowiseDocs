---
description: 了解如何使用 Flowise 的外部 API 集成
---

# 与 API 交互

***

OpenAPI 规范 (OAS) 定义了 HTTP API 的标准、与语言无关的接口。此用例的目标是让 LLM 自动确定要调用哪个 API，同时仍与用户进行有状态对话。

## OpenAPI 链

1. 在本教程中，我们将使用 [Klarna OpenAPI](https://gist.github.com/HenryHengZJ/b60f416c42cb9bcd3160fe797421119a)

{% code overflow="wrap" %}
```json
{
  "openapi": "3.0.1",
  "info": {
    "version": "v0",
    "title": "Open AI Klarna product Api"
  },
  "servers": [
    {
      "url": "https://www.klarna.com/us/shopping"
    }
  ],
  "tags": [
    {
      "name": "open-ai-product-endpoint",
      "description": "Open AI Product Endpoint. Query for products."
    }
  ],
  "paths": {
    "/public/openai/v0/products": {
      "get": {
        "tags": [
          "open-ai-product-endpoint"
        ],
        "summary": "API for fetching Klarna product information",
        "operationId": "productsUsingGET",
        "parameters": [
          {
            "name": "countryCode",
            "in": "query",
            "description": "ISO 3166 country code with 2 characters based on the user location. Currently, only US, GB, DE, SE and DK are supported.",
            "required": true,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "q",
            "in": "query",
            "description": "A precise query that matches one very small category or product that needs to be searched for to find the products the user is looking for. If the user explicitly stated what they want, use that as a query. The query is as specific as possible to the product name or category mentioned by the user in its singular form, and don't contain any clarifiers like latest, newest, cheapest, budget, premium, expensive or similar. The query is always taken from the latest topic, if there is a new topic a new query is started. If the user speaks another language than English, translate their request into English (example: translate fia med knuff to ludo board game)!",
            "required": true,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "size",
            "in": "query",
            "description": "number of products returned",
            "required": false,
            "schema": {
              "type": "integer"
            }
          },
          {
            "name": "min_price",
            "in": "query",
            "description": "(Optional) Minimum price in local currency for the product searched for. Either explicitly stated by the user or implicitly inferred from a combination of the user's request and the kind of product searched for.",
            "required": false,
            "schema": {
              "type": "integer"
            }
          },
          {
            "name": "max_price",
            "in": "query",
            "description": "(Optional) Maximum price in local currency for the product searched for. Either explicitly stated by the user or implicitly inferred from a combination of the user's request and the kind of product searched for.",
            "required": false,
            "schema": {
              "type": "integer"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "Products found",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProductResponse"
                }
              }
            }
          },
          "503": {
            "description": "one or more services are unavailable"
          }
        },
        "deprecated": false
      }
    }
  },
  "components": {
    "schemas": {
      "Product": {
        "type": "object",
        "properties": {
          "attributes": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "name": {
            "type": "string"
          },
          "price": {
            "type": "string"
          },
          "url": {
            "type": "string"
          }
        },
        "title": "Product"
      },
      "ProductResponse": {
        "type": "object",
        "properties": {
          "products": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/Product"
            }
          }
        },
        "title": "ProductResponse"
      }
    }
  }
}
```
{% endcode %}

2. 您可以使用 [JSON 到 YAML 转换器](https://jsonformatter.org/json-to-yaml) 并将其保存为 `.yaml` 文件，并将其上传到 **OpenAPI Chain**，然后通过提出一些问题进行测试。 **OpenAPI Chain** 会将整个规范发送到 LLM，并让 LLM 自动使用正确的方法和参数进行 API 调用。

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

3.但是，如果你想进行正常的对话聊天，则不能够这样做。您将看到以下错误。这是因为OpenAPI Chain有如下提示词：

```
Use the provided API's to respond to this user query
```

由于我们“强制”它始终找到 API 来回答用户查询，因此在与 OpenAPI 无关的正常对话的情况下，它无法这样做。

<figure><img src="../.gitbook/assets/image (134).png" alt="" width="361"><figcaption></figcaption></figure>

如果您有较大的 OpenAPI 规范，则使用此方法可能效果不佳。这是因为我们将所有规范作为发送到 LLM 的消息的一部分包含在内。然后，我们依靠 LLM 找出正确的 URL、查询参数、请求正文以及回答用户查询所需的其他必要参数。正如您可以想象的那样，如果您的 OpenAPI 规范很复杂，那么 LLM 产生幻觉的可能性就更高。

## 工具代理+OpenAPI工具包

为了解决上述错误，我们可以使用Agent。根据 OpenAI 的官方说明书：[使用 OpenAPI 规范调用函数](https://cookbook.openai.com/examples/function_calling_with_an_openapi_spec)，建议将每个 API 转换为工具本身，而不是将所有 API 作为单个消息输入到 LLM 中。代理还能够进行类似人类的交互，能够根据用户的查询决定使用哪个工具。

OpenAPI Toolkit 会将 YAML 文件中的每个 API 转换为一组工具。这样，用户就不必为每个 API 创建 [自定义工具](../integrations/langchain/tools/custom-tool.md)。

1. 将 **ToolAgent** 与 **OpenAPI Toolkit** 连接。在这里，我们上传 OpenAI API 的 YAML 规范。规格文件可以在页面底部找到。

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

2.我们来试试吧！

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

从聊天中可以看出，座席能够进行正常的对话，并使用适当的工具来回答用户的询问。如果您使用的是分析工具，您可以看到我们从 YAML 文件转换的工具列表：

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

## 结论

我们已经成功创建了一个代理，可以在必要时与 API 交互，并且仍然能够处理与用户的有状态对话。以下是本节中使用的模板：

{% file src="../.gitbook/assets/OpenAPI Chatflow.json" %}

{% file src="../.gitbook/assets/OpenAPI Toolkit with ToolAgent Chatflow.json" %}

{% file src="../.gitbook/assets/openai_openapi.yaml" %}
