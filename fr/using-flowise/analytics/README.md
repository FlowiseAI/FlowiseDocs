---
description: Learn how to analyze and troubleshoot your chatflows and agentflows
---

# Analytique

***

Flowise fournit un traçage étape par étape pour[Agentflow V2](../agentflowv2.md):

<gigne> <img src = "../../. GitBook / Assets / Image (332) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

En outre, il existe également plusieurs fournisseurs d'analyse qui circulent avec:

* [LunaryAI](https://lunary.ai/)
* [Langsmith](https://smith.langchain.com/)
* [Langfuse](https://langfuse.com/)
* [LangWatch](https://langwatch.ai/)
* [Arize](https://arize.com/)
* [Phoenix](https://phoenix.arize.com/)
* [Opik](https://www.comet.com/site/products/opik/)

## Installation

1. Dans le coin supérieur droit de votre ChatFlow ou AgentFlow, cliquez sur ** Paramètres **> ** Configuration **

<gigne> <img src = "../../. GitBook / Assets / analytic-1.webp" alt = "Capture d'écran de l'utilisateur cliquant dans le menu de configuration" width = "375"> <figCaption> </gigcaption> </ figure>

2. Ensuite, allez à la section Analyser Chatflow

<Figure> <img src = "../../. GitBook / Assets / Analytic-2.png" alt = "Capture d'écran de la section Analyze Chatflow avec les différents fournisseurs d'analyses"> <FigCaption> </gigcaption> </ figure>

3. Vous verrez une liste des fournisseurs, ainsi que leurs champs de configuration

<gigne> <img src = "../../. GitBook / Assets / Image (82) .png" alt = "ScreenShot d'un fournisseur d'analyse avec des champs d'identification étendus"> <Figcaption> </Gigcaption> </ Figure>

4. Remplissez les informations d'identification et autres détails de configuration, puis allumez le fournisseur ** sur **. Cliquez sur Enregistrer.

<gigne> <img src = "../../. GitBook / Assets / Image (83) .png" alt = "ScreenShot of Analytics Providers activé"> <Figcaption> </ Figcaption> </gistre>

## API

Une fois l'analytique allumé de l'interface utilisateur, vous pouvez remplacer ou fournir une configuration supplémentaire dans le corps du[Prediction API](api.md#prediction-api):

```json
{
  "question": "hi there",
  "overrideConfig": {
    "analytics": {
      "langFuse": {
        // langSmith, langFuse, lunary, langWatch, opik
        "userId": "user1"
      }
    }
  }
}
```
