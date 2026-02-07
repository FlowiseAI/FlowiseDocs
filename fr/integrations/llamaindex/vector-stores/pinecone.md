---
description: >-
  Upsert embedded data and perform similarity search upon query using Pinecone,
  a leading fully managed hosted vector database.
---

# Pignon

## Condition préalable

1. Enregistrer un compte pour[Pinecone](https://app.pinecone.io/)
2. Cliquez sur ** Créer un index **

<gigne> <img src = "../../../. GitBook / Assets / PineCone_1.png" alt = ""> <Figcaption> </gigcaption> </gigust>

3. Remplissez les champs requis:
   * ** Nom de l'index **, nom de l'index à créer. (par exemple, "test fluide")
   * ** Dimensions **, taille des vecteurs à insérer dans l'index. (par exemple 1536)

<gigne> <img src = "../../../. GitBook / Assets / PineCone_2.png" alt = "" width = "527"> <Figcaption> </ Figcaption> </ Figure>

4. Cliquez sur ** Créer un index **

## Installation

1. Obtenez / créez votre ** clé API **

<gigne> <img src = "../../../. GitBook / Assets / PineCone_3.png" alt = ""> <Figcaption> </gigcaption> </gigne>

2. Ajoutez un nouveau nœud ** Pinecone ** à Canvas et remplissez les paramètres:
   * Index de pince
   * Espace de noms PineCone (facultatif)

<gigne> <img src = "../../../. GitBook / Assets / PineCone_Llamaindex.png" alt = "" width = "301"> <Figcaption> <p> Pinecone Node </p> </gigcaption> </pigucial>

3. Créer un nouvel identifiant PineCone -> Remplissez ** Clé API **

<gigne> <img src = "../../../. GitBook / Assets / PineCone_5.png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

4. Ajouter des nœuds supplémentaires à la toile et démarrer le processus ussert
   *   ** Document ** peut être connecté à n'importe quel nœud sous[**Document Loader**](../../langchain/document-loaders/)catégorie

{% hint style = "info"%}
Les chargeurs de documents et les séparateurs de texte pour Llamaindex ne sont pas encore disponibles, mais l'utilisation de l'une de celles disponibles sous Langchain permettra toujours la question avec Llamaindex comme d'habitude.
{% EndHint%}

\ - \ * \ * Embeddings \ * \ * peut être connecté à n'importe quel nœud sous \ [\ * \ * Embeddings \ * \ *] \ (../ Embeddings /)

<gigne> <img src = "../../../. Gitbook / Assets / PineCone_llama_Chatflow.png" alt = ""> <Figcaption> </ Figcaption> </ Figure>

<gigne> <img src = "../../../. Gitbook / Assets / PineCone_llama_upsert.png" alt = ""> <figcaption> </ Figcaption> </ Figure>

5. Vérifier sur[Pinecone dashboard](https://app.pinecone.io)Ces données ont été renversées avec succès:

<gigne> <img src = "../../../. Gitbook / Assets / PineCone_8.png" alt = ""> <Figcaption> </gigcaption> </gigne>
