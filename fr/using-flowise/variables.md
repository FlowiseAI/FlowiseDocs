---
description: Learn how to use variables in Flowise
---

# Variables

***

Flowise permet aux utilisateurs de créer des variables qui peuvent être utilisées dans les nœuds. Les variables peuvent être statiques ou d'exécution.

### Statique

La variable statique sera enregistrée avec la valeur spécifiée et récupérée telle qu'elle est.

<gigne> <img src = "../. Gitbook / Assets / image (13) (1) (1) (1) (1) (1) .png" alt = "" width = "542"> <figCaption> </ Figcaption> </gigu

### Temps d'exécution

La valeur de la variable sera récupérée à partir du fichier **. Env ** en utilisant`process.env`

<Figure> <img src = "../. Gitbook / Assets / Image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "537"> <Figcaption> </gigcaption> </gigust>

### Remplacer ou régler la variable via l'API

Afin de remplacer la valeur de la variable, l'utilisateur doit l'activer explicitement à partir du bouton supérieur droit:

** Paramètres ** -> ** Configuration ** -> ** Sécurité ** Tab:

<gigne> <img src = "../. GitBook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = ""> <Figcaption> </ Figcaption> </ / Figure>

S'il existe une variable existante créée, la valeur de variable fournie dans l'API remplacera la valeur existante.

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

### En utilisant des variables

Les variables peuvent être utilisées par les nœuds en flux. Par exemple, une variable nommée **`character`** est créé:

<gigne> <img src = "../. GitBook / Assets / Image (96) .png" alt = ""> <figcaption> </gigcaption> </gigust>

Nous pouvons alors utiliser cette variable comme **`$vars.<variable-name>`** Dans la fonction des nœuds suivants:

* [Custom Tool](../integrations/langchain/tools/custom-tool.md)
* [Custom Function](../integrations/utilities/custom-js-function.md)
* [Custom Loader](../integrations/langchain/document-loaders/custom-document-loader.md)
* [If Else](../integrations/utilities/if-else.md)
* MCP personnalisé

<gigne> <img src = "../. GitBook / Assets / Image (105) .png" alt = "" width = "283"> <figcaption> </gigcaption> </gigust>

En outre, l'utilisateur peut également utiliser la variable dans l'entrée de texte de n'importe quel nœud avec le format suivant:

**`{{$vars.<variable-name>}}`**

Par exemple, dans le message du système d'agent:

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (2) (1) .png" alt = "" width = "508"> <figcaption> </gigcaption> </gigu

Dans le modèle invite:

<gigne> <img src = "../. GitBook / Assets / Image (157) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

## Ressources

* [Pass Variables to Function](../integrations/langchain/tools/custom-tool.md#pass-variables-to-function)
