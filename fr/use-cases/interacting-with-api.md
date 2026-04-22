---
description: Learn how to use external API integrations with Flowise
---

# Interagir avec l'API

***

La spécification OpenAPI (OAS) définit une interface standard et agnostique linguistique aux API HTTP. L'objectif de ce cas d'utilisation est que le LLM détermine automatiquement quelle API appelle, tout en ayant une conversation avec état avec l'utilisateur.

## Chaîne OpenAPI

1. Dans ce tutoriel, nous allons utiliser[Klarna OpenAPI](https://gist.github.com/HenryHengZJ/b60f416c42cb9bcd3160fe797421119a)

{% code overflow = "wrap"%}
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
{% Endcode%}

2. Vous pouvez utiliser un[JSON to YAML converter](https://jsonformatter.org/json-to-yaml)et enregistrer comme un`.yaml`Fixez et téléchargez-le sur ** OpenAPI Chain **, puis testez en posant des questions. ** OpenAPI Chain ** Enverra toutes les spécifications à LLM et que le LLM utilise automatiquement la méthode et les paramètres corrects pour l'appel API.

<gigne> <img src = "../. GitBook / Assets / Image (133) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

3. Cependant, si vous voulez avoir un chat de conversation normal, il ne peut pas le faire. Vous verrez l'erreur suivante. En effet, la chaîne OpenAPI a l'invite suivante:

```
Use the provided API's to respond to this user query
```

Puisque nous avons "forcé" qu'il trouve toujours l'API pour répondre à la requête utilisateur, dans les cas d'une conversation normale qui n'est pas pertinente pour l'OpenAPI, il ne le fait pas.

<gigne> <img src = "../. GitBook / Assets / Image (134) .png" alt = "" width = "361"> <figcaption> </gigcaption> </ figure>

L'utilisation de cette méthode peut ne pas fonctionner bien si vous avez de grandes spécifications OpenAPI. En effet, nous incluons toutes les spécifications dans le cadre du message envoyé à LLM. Nous comptons ensuite sur LLM pour déterminer l'URL, les paramètres de requête corrects, le corps de demande et les autres paramètres nécessaires nécessaires pour répondre à la requête utilisateur. Comme vous pouvez l'imaginer, si vos spécifications OpenAPI sont compliquées, il y a un risque plus élevé que LLM hallucine.

## Agent d'outils + boîte à outils OpenAPI

Afin de résoudre l'erreur ci-dessus, nous pouvons utiliser l'agent. Du livre de cuisine officiel d'Openai:[Function calling with an OpenAPI specification](https://cookbook.openai.com/examples/function_calling_with_an_openapi_spec), il est recommandé de convertir chaque API en un outil lui-même, au lieu de nourrir toutes les API en LLM en tant que message unique. Un agent est également capable d'avoir une interaction humaine, avec la possibilité de décider quel outil utiliser en fonction de la requête de l'utilisateur.

OpenAPI Toolkit convertira chacun des API du fichier YAML en un ensemble d'outils. De cette façon, les utilisateurs n'ont pas à créer un[Custom Tool](../integrations/langchain/tools/custom-tool.md)pour chaque API.

1. Connectez ** Toolagent ** avec ** OpenAPI Toolkit **. Ici, nous téléchargeons la spécification YAML pour l'API OpenAI. Le fichier de spécifications peut être trouvé en bas de la page.

<gigne> <img src = "../. GitBook / Assets / Image (25) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

2. Essayons-le!

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (2) .png" alt = ""> <figcaption> </gigcaption> </stigual>

Comme vous pouvez le remarquer à partir du chat, l'agent est capable de mener une conversation normale et d'utiliser un outil approprié pour répondre à la requête utilisateur. Si vous utilisez l'outil analytique, vous pouvez voir la liste des outils que nous avons convertis à partir du fichier YAML:

<gigne> <img src = "../. Gitbook / Assets / image (2) (1) (1) (1) (1) (1) (2) (1) .png" alt = ""> <figCaption> </gigcaption> </gigu

## Conclusion

Nous avons réussi à créer un agent capable d'interagir avec l'API si nécessaire, tout en étant capable de gérer les conversations avec état avec les utilisateurs. Voici les modèles utilisés dans cette section:

{% fichier src = "../. gitbook / actifs / openapi chatflow.json"%}

{% fichier src = "../. GitBook / Assets / OpenAPI Toolkit avec ToolAgent Chatflow.json"%}

{% fichier src = "../. gitbook / actifs / openai_openapi.yaml"%}
