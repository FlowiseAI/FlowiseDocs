---
description: Learn how to use external API integrations with Flowise
---

# Interacting with API

***

The OpenAPI Specification (OAS) defines a standard, language-agnostic interface to HTTP APIs. The goal of this use case is to have the LLM automatically figure out which API to call, while still having a stateful conversation with user.

## OpenAPI Chain

1. In this tutorial, we are going to use [Klarna OpenAPI](https://gist.github.com/HenryHengZJ/b60f416c42cb9bcd3160fe797421119a)

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

2. You can use a [JSON to YAML converter](https://jsonformatter.org/json-to-yaml) and save it as a `.yaml` file, and upload it to **OpenAPI Chain**, then test by asking some questions. **OpenAPI Chain** will send the whole specs to LLM, and have the LLM automatically use the correct method and parameters for the API call.

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

3. However, if you want to have a normal conversation chat, it is not able to do so. You will see the following error. This is because OpenAPI Chain has the following prompt:

```
Use the provided API's to respond to this user query
```

Since we "forced" it to always find the API to answer user query, in the cases of normal conversation that is irrelevant to the OpenAPI, it fails to do so.

<figure><img src="../.gitbook/assets/image (134).png" alt="" width="361"><figcaption></figcaption></figure>

Using this method might not work well if you have large OpenAPI spec. This is because we are including all the specifications as part of the message sent to LLM. We then rely on LLM to figure out the correct URL, query parameters, request body, and other necessary parameters needed to answer user query. As you can imagine, if your OpenAPI specs are complicated, there is a higher chance LLM will hallucinates.

## Tool Agent + OpenAPI Toolkit

In order to solve the above error, we can use Agent. From the official cookbook by OpenAI: [Function calling with an OpenAPI specification](https://cookbook.openai.com/examples/function\_calling\_with\_an\_openapi\_spec), it is recommended to convert each API into a tool itself, instead of feeding all the APIs into LLM as single message. An agent is also capable of having human-like interaction, with the ability to decide which tool to use depending on user's query.

OpenAPI Toolkit will converts each of the API from YAML file into a set of tools. This way, users don't have to create a [Custom Tool](../integrations/langchain/tools/custom-tool.md) for each API.

1. Connect **ToolAgent** with **OpenAPI Toolkit**. Here, we upload the YAML spec for OpenAI API. The spec file can be found at the bottom of the page.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

2. Let's try it!

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

As you can noticed from the chat, the agent is capable of carrying out normal conversation, and use appropriate tool to answer user query. If you are using Analytic Tool, you can see the list of tools we converted from the YAML file:

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Conclusion

We've successfully created an agent that can interact with API when necessary, and still be able handle stateful conversations with users. Below are the templates used in this section:

{% file src="../.gitbook/assets/OpenAPI Chatflow.json" %}

{% file src="../.gitbook/assets/OpenAPI Toolkit with ToolAgent Chatflow.json" %}

{% file src="../.gitbook/assets/openai_openapi.yaml" %}
