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
   - ** Nom de l'index **, nom de l'index à créer. (par exemple, "test fluide")
   - ** Dimensions **, taille des vecteurs à insérer dans l'index. (par exemple 1536)

<gigne> <img src = "../../../. GitBook / Assets / PineCone_2.png" alt = "" width = "527"> <Figcaption> </ Figcaption> </ Figure>

4. Cliquez sur ** Créer un index **

## Installation

1.  Obtenez / créez votre ** clé API **

<gigne> <img src = "../../../. GitBook / Assets / PineCone_3.png" alt = ""> <Figcaption> </gigcaption> </gigne>

2.  Ajoutez un nouveau nœud ** Pinecone ** à Canvas et remplissez les paramètres:
    - Index de pince
    - Espace de noms PineCone (facultatif)

<gigne> <img src = "../../../. GitBook / Assets / PineCone_4.png" alt = "" width = "279"> <Figcaption> </gigcaption> </gigust>

3. Créer un nouvel identifiant PineCone -> Remplissez ** Clé API **

<gigne> <img src = "../../../. GitBook / Assets / PineCone_5.png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

4. Ajouter des nœuds supplémentaires à la toile et démarrer le processus ussert
   - ** Document ** peut être connecté à n'importe quel nœud sous[**Document Loader**](../document-loaders/)catégorie
   - ** Embeddings ** peut être connecté à n'importe quel nœud sous[**Embeddings** ](../embeddings/)catégorie

<gigne> <img src = "../../../. Gitbook / Assets / PineCone_6.png" alt = ""> <Figcaption> </gigcaption> </ Figure>

<gigne> <img src = "../../../. GitBook / Assets / PineCone_7.png" alt = ""> <Figcaption> </gigcaption> </gigne>

5. Vérifier[Pinecone dashboard](https://app.pinecone.io)Pour voir si les données ont été renversées avec succès:

<gigne> <img src = "../../../. Gitbook / Assets / PineCone_8.png" alt = ""> <Figcaption> </gigcaption> </gigne>

6.

## Ressources

- Intégrations Langchain Pinecone Vectorstore
  - [Python](https://python.langchain.com/v0.2/docs/integrations/providers/pinecone/)
  - [NodeJS](https://js.langchain.com/v0.2/docs/integrations/vectorstores/pinecone)
- [Pinecone LangChain integration](https://docs.pinecone.io/integrations/langchain)
- [Pinecone Flowise integration](https://docs.pinecone.io/integrations/flowise)
- [Pinecone official clients](https://docs.pinecone.io/reference/pinecone-clients)
