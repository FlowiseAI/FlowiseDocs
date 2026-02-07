---
description: Custom Retriever allows user to specify the format of the context to LLM
---

# Retriever personnalisé

<gigne> <img src = "../../../. GitBook / Assets / Image (3) (1) (1) (2) (1) .png" alt = "" width = "298"> <figCaption> </ Figcaption> </ Figure>

Par défaut, lorsque le contexte est récupéré à partir du magasin Vector, ils sont dans le format suivant:

```json
[ 
    {
        "pageContent": "This is an example",
        "metadata": {
            "source": "example.pdf"
        }
    },
    {
        "pageContent": "This is example 2",
        "metadata": {
            "source": "example2.txt"
        }
    }
]
```

** PageContent ** du tableau sera assemblé en tant que chaîne et renvoyé à LLM pour l'achèvement.

Cependant, dans certains cas, vous voudrez peut-être inclure des informations provenant de métadonnées pour donner plus d'informations à LLM, telles que Source, Link, etc. C'est là que ** personnalisé Retriever ** entre en jeu. Nous pouvons spécifier le format pour revenir à LLM.

Par exemple, en utilisant le format suivant:

```javascript
{{context}}
Source: {{metadata.source}}
```

Entraînera la chaîne combinée comme ci-dessous:

```
This is an example
Source: example.pdf

This is example 2
Source: example2.txt
```

Cela sera renvoyé à LLM. Étant donné que LLM possède désormais les sources des réponses, nous pouvons utiliser des invites pour instruire LLM de retourner des réponses suivies de citations.
