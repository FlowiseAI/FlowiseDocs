# Vectara

## Tutoriel QuickStart

{% embed url = "https://www.youtube.com/watch?v=rbqpvfcd5xy"%}

## Condition préalable

1. Enregistrer un compte pour[Vectara](https://vectara.com/integrations/flowise)
2. Cliquez sur ** Créer un corpus **

<gigne> <img src = "../../../. Gitbook / Assets / Vectara / 1.png" alt = ""> <Figcaption> </gigcaption> </gigust>

Nommez le corpus à créer et cliquez sur ** Créer un corpus ** puis attendez que le corpus termine la configuration.

## Installation

1. Cliquez sur l'onglet ** "Contrôle d'accès" ** dans la vue du corpus

<gigne> <img src = "../../../. Gitbook / Assets / Vectara / 2.png" alt = ""> <Figcaption> </gigcaption> </gigust>

2. Cliquez sur le bouton ** "Créer la touche API" **, choisissez un nom pour la touche API et choisissez l'option ** QuerryService & indexService **

<gigne> <img src = "../../../. GitBook / Assets / Vectara / 3.png" alt = ""> <Figcaption> </gigcaption> </gigne>

3. Cliquez sur ** Créer ** pour créer la touche API
4. Obtenez votre ID ** Corpus, votre clé API et votre ID client ** en cliquant sur la flèche vers le bas sous "Copie" pour votre nouvelle clé API:

<gigne> <img src = "../../../. GitBook / Assets / Vectara / 4.png" alt = ""> <Figcaption> </ Figcaption> </gigne>

5. Retour à Flowise Canvas et créez votre ChatFlow. Cliquez sur ** Créer un nouveau ** à partir de la liste déroulante des informations d'identification ANE Entrez vos informations d'identification Vectara.

<gigne> <img src = "../../../. GitBook / Assets / Vectara / 5.png" alt = "" width = "500"> <Figcaption> </ Figcaption> </ Figure>

6. Apprécier!

## Paramètres de requête vectara

Pour un contrôle plus fin sur les paramètres de requête Vectara, cliquez sur "** Paramètres supplémentaires **", puis vous pouvez mettre à jour les paramètres suivants à partir de leur défaut:

* Filtre de métadonnées: Vectara prend en charge le filtrage des méta-données. Pour utiliser[filtering](https://docs.vectara.com/docs/common-use-cases/filtering-by-metadata/filter-overview), assurez-vous que les champs de métadonnées que vous souhaitez filtrer sont définis dans votre corpus vectara.
* "Sentiments avant" et "phrases après": Celles-ci contrôlent combien de phrases avant / après le texte de correspondance sont renvoyées en tant que résultats du moteur de récupération de Vectara
* Lambda: définit le comportement de[hybrid search](https://docs.vectara.com/docs/learn/hybrid-search)à Vectara
* Top-K: Combien de résultats retourner de Vectara pour la requête
* MMR-K: nombre de résultats à utiliser pour[MMR](https://docs.vectara.com/docs/api-reference/search-apis/reranking#maximal-marginal-relevance-mmr-reranker)(Relvance marginale maximale)

<gigne> <img src = "../../../. gitbook / actifs / vectara / 6.png" alt = "" width = "500"> <Figcaption> </gigcaption> </ figure>

## Ressources

* [LangChain JS Vectara Blog Post](https://blog.langchain.dev/langchain-vectara-better-together/)
* [5 Reasons to Use Vectara's Langchain Integration Blog Post](https://vectara.com/5-reasons-to-use-vectaras-langchain-integration/)
* [Max Marginal Relevance in Vectara](https://vectara.com/blog/get-diverse-results-and-comprehensive-summaries-with-vectaras-mmr-reranker/)
* [Vectara Boomerang embedding model Blog Post](https://vectara.com/introducing-boomerang-vectaras-new-and-improved-retrieval-model/)
* [Detecting Hallucination with Vectara's HHEM](https://vectara.com/blog/cut-the-bull-detecting-hallucinations-in-large-language-models/)
