---
description: Aprende a usar integraciones de API externas con Flowise
---

# Interactuando con API

***

La OpenAPI Specification (OAS) define una interfaz estándar e independiente del lenguaje para APIs HTTP. El objetivo de este caso de uso es que el LLM determine automáticamente qué API llamar, mientras mantiene una conversación con estado con el usuario.

## OpenAPI Chain

1. En este tutorial, vamos a usar [Klarna OpenAPI](https://gist.github.com/HenryHengZJ/b60f416c42cb9bcd3160fe797421119a)

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

2. Puedes usar un [conversor de JSON a YAML](https://jsonformatter.org/json-to-yaml) y guardarlo como archivo `.yaml`, y subirlo a **OpenAPI Chain**, luego probar haciendo algunas preguntas. **OpenAPI Chain** enviará todas las especificaciones al LLM, y hará que el LLM use automáticamente el método y los parámetros correctos para la llamada a la API.

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

3. Sin embargo, si quieres tener una conversación normal, no podrá hacerlo. Verás el siguiente error. Esto es porque OpenAPI Chain tiene el siguiente prompt:

```
Use the provided API's to respond to this user query
```

Como lo "forzamos" a siempre encontrar la API para responder la consulta del usuario, en los casos de conversación normal que no son relevantes para la OpenAPI, falla al hacerlo.

<figure><img src="../.gitbook/assets/image (134).png" alt="" width="361"><figcaption></figcaption></figure>

Usar este método podría no funcionar bien si tienes una especificación OpenAPI grande. Esto es porque estamos incluyendo todas las especificaciones como parte del mensaje enviado al LLM. Luego dependemos del LLM para determinar la URL correcta, los parámetros de consulta, el cuerpo de la solicitud y otros parámetros necesarios para responder la consulta del usuario. Como puedes imaginar, si tus especificaciones OpenAPI son complicadas, hay una mayor probabilidad de que el LLM alucine.

## Tool Agent + OpenAPI Toolkit

Para resolver el error anterior, podemos usar un Agent. Según el cookbook oficial de OpenAI: [Function calling with an OpenAPI specification](https://cookbook.openai.com/examples/function_calling_with_an_openapi_spec), se recomienda convertir cada API en una tool por sí misma, en lugar de alimentar todas las APIs al LLM como un solo mensaje. Un agent también es capaz de tener una interacción similar a la humana, con la capacidad de decidir qué tool usar dependiendo de la consulta del usuario.

OpenAPI Toolkit convertirá cada una de las APIs del archivo YAML en un conjunto de tools. De esta manera, los usuarios no tienen que crear una [Custom Tool](../integrations/langchain/tools/custom-tool.md) para cada API.

1. Conecta **ToolAgent** con **OpenAPI Toolkit**. Aquí, subimos la especificación YAML para la API de OpenAI. El archivo de especificaciones se puede encontrar al final de la página.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

2. ¡Vamos a probarlo!

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Como puedes notar en el chat, el agent es capaz de mantener una conversación normal y usar la tool apropiada para responder la consulta del usuario. Si estás usando Analytic Tool, puedes ver la lista de tools que convertimos del archivo YAML:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Conclusión

Hemos creado exitosamente un agent que puede interactuar con API cuando es necesario y aún así poder manejar conversaciones con estado con los usuarios. A continuación se encuentran las plantillas utilizadas en esta sección:

{% file src="../.gitbook/assets/OpenAPI Chatflow.json" %}

{% file src="../.gitbook/assets/OpenAPI Toolkit with ToolAgent Chatflow.json" %}

{% file src="../.gitbook/assets/openai_openapi.yaml" %}
