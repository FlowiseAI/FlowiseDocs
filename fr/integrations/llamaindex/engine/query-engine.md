# Moteur de requête

Un moteur de requête sert de pipeline de bout en bout permet aux utilisateurs de poser des questions sur leurs données. Il reçoit une requête en langue naturelle et fournit une réponse, accompagnée d'informations de contexte pertinentes récupérées et transmises au LLM (modèle de grande langue).

<gigne> <img src = "../../../. GitBook / Assets / Image (3) (1) (1) (1) (1) (1) (1) (2) (1) .png" alt = ""> <figCaption> </FigCaption> </pigucial>

## Entrées

* Retriever du magasin vectoriel
* [Response Synthesizer](../response-synthesizer/)

## Paramètres

| Nom | Description |
| ----------------------- | ------------------------------------------------------------------- |
| Retour des documents source | Pour retourner les citations / sources qui ont été utilisées pour construire la réponse |

## Sorties

| Nom | Description |
| ----------- | ----------------------------- |
| QueryEngine | Node final pour retourner la réponse |
