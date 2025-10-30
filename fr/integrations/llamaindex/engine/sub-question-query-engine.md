# Moteur de requête de sous-question

Un moteur de requête conçu pour résoudre le problème de la réponse à une requête complexe à l'aide de plusieurs sources de données. Il décompose d'abord la requête complexe en sous-questions pour chaque source de données pertinente, puis recueille toutes les repres intermédiaires et synthétise une réponse finale.

<gigne> <img src = "../../../. GitBook / Assets / Image (4) (1) (1) (1) (1) (2) (1) .png" alt = ""> <figCaption> </gigcaption> </ figure>

## Entrées

* Outils de moteur de requête
* Modèle de chat
* Incorporer
* [Response Synthesizer](../response-synthesizer/)

## Paramètres

| Nom | Description |
| ----------------------- | ------------------------------------------------------------------- |
| Retour des documents source | Pour retourner les citations / sources qui ont été utilisées pour construire la réponse |

## Sorties

| Nom | Description |
| ---------------------- | ----------------------------- |
| SUBESTIONQUESQUEURYEngine | Node final pour retourner la réponse |
