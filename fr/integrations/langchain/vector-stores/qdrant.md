# Qdrant

## Condition préalable

UN[locally running instance of Qdrant](https://qdrant.tech/documentation/quick-start/)ou une instance de cloud Qdrant.

Pour obtenir une instance de cloud Qdrant:

1. Dirigez-vous vers la section des grappes du[Cloud Dashboard](https://cloud.qdrant.io/overview).
2. Sélectionnez ** Clusters ** puis cliquez sur ** + Créer **.

<gigne> <img src = "../../../. GitBook / Assets / Qdrant / 2.png" alt = ""> <Figcaption> </gigcaption> </gigne>

3. Choisissez vos configurations de cluster et votre région.
4. Appuyez sur ** Créer ** pour provisionner votre cluster.

## Installation

1. Obtenez / créez votre ** clé API ** à partir de la section ** Contrôle d'accès aux données ** de la[Cloud Dashboard](https://cloud.qdrant.io/overview).
2. Ajoutez un nouveau nœud ** QDRANT ** sur Canvas.
3. Créez de nouveaux informations d'identification QDRANT à l'aide de la clé API

<gigne> <img src = "../../../. GitBook / Assets / Qdrant / 1.png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

4. Entrez les informations requises dans le nœud ** qdrant **:
   * URL du serveur Qdrant
   * Nom de collection

<gigne> <img src = "../../../. GitBook / Assets / Qdrant / 3.png" alt = "" width = "239"> <Figcaption> </gigcaption> </gigust>

5. ** Document ** L'entrée peut être connectée à n'importe quel nœud sous[**Document Loader**](../document-loaders/)catégorie.
6. ** Embeddings ** L'entrée peut être connectée à n'importe quel nœud sous[**Embeddings**](../embeddings/)catégorie.

## Filtration

Disons que vous avez différents documents renversés, chacun spécifié avec une valeur unique sous la clé de métadonnées`{source}`

<div align = "Left">

<gigne> <img src = "../../../. GitBook / Assets / Capture 2024-03-05 141551.png" alt = "" width = "563"> <figcaption> </ figCaption> </ Figure>

 

<gigne> <img src = "../../../. GitBook / Assets / Capture 2024-03-05 141619.png" alt = "" width = "563"> <Figcaption> </gigCaption> </ Figure>

</div>

Ensuite, vous voulez filtrer par cela. Qdrant prend en charge la suite[syntax](https://qdrant.tech/documentation/concepts/filtering/#nested-key)En ce qui concerne le filtrage:

** ui **

<gigne> <img src = "../../../. Gitbook / Assets / Image (1) (1) (1) (1) (1) (1) (1) (2) (1) (1) (1) .png" alt = "" width = "338"> <gigcaption>

** API **

```json
"overrideConfig": {
    "qdrantFilter": {
        "should": [
            {
                "key": "metadata.source",
                "match": {
                    "value": "apple"
                }
            }
        ]
    }
}
```

## Ressources

* [Qdrant documentation](https://qdrant.tech/documentation/)
* [LangChain JS Qdrant](https://js.langchain.com/docs/integrations/vectorstores/qdrant)
* [Qdrant Filter](https://qdrant.tech/documentation/concepts/filtering/#nested-key)
