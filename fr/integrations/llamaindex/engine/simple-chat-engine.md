# Moteur de chat simple

Un moteur de chat simple fonctionne comme un pipeline complet pour s'engager dans un dialogue entre l'IA et l'utilisateur, sans récupération de contexte. Cependant, il est équipé de[Memory](../../langchain/memory/), permettant de se souvenir des conversations.

<gigne> <img src = "../../../. GitBook / Assets / Image (2) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = ""> <figCaption> </gigCaption> </pigucial>

## Entrées

* Modèle de chat
* [Memory](../../langchain/memory/)

## Paramètres

| Nom | Description |
| -------------- | --------------------------------------------- |
| Message système | Une instruction pour LLM sur la façon de répondre à la requête |

## Sorties

| Nom | Description |
| ---------------- | ----------------------------- |
| Simplechatégine | Node final pour retourner la réponse |
