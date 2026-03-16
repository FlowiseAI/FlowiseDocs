# Moteur de chat contextuel

Un moteur de chat sert de pipeline de bout en bout pour avoir une conversation humaine avec vos données, permettant plusieurs échanges plutôt qu'une seule interaction de questions et réponses.

<gigne> <img src = "../../../. Gitbook / Assets / Image (3) (1) (1) (1) (1) (1) (1) (2) (1) (1) .png" alt = ""> <figCaption> </gigcaption> </pigucial>

## Entrées

* Modèle de chat
* Retriever du magasin vectoriel
* [Memory](../../langchain/memory/)

## Paramètres

| Nom | Description |
| ----------------------- | ------------------------------------------------------------------- |
| Retour des documents source | Pour retourner les citations / sources qui ont été utilisées pour construire la réponse |
| Message système | Une instruction pour LLM sur la façon de répondre à la requête |

## Sorties

| Nom | Description |
| ----------------- | ----------------------------- |
| ContextChatengine | Node final pour retourner la réponse |
