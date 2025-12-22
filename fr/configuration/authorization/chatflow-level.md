---
description: Apprenez à configurer le contrôle d'accès au niveau du chatflow pour vos instances Flowise
---

# Flows

***

Après avoir construit un chatflow / agentflow, par défaut, votre flux est accessible au public. Quiconque ayant accès à l'ID du Chatflow peut exécuter des prédictions via Embed ou API.

Dans les cas où vous souhaitez permettre à certaines personnes d'accéder et d'interagir avec celui-ci, vous pouvez le faire en attribuant une clé API pour ce chatflow spécifique.

## Clé API

Dans le tableau de bord, accédez à la section Clés API, et vous devriez voir une DefaultKey créée. Vous pouvez également ajouter ou supprimer des clés.

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Chatflow

Accédez au chatflow, et maintenant vous pouvez sélectionner la clé API que vous souhaitez utiliser pour protéger le chatflow.

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Après avoir attribué une clé API, on ne peut accéder à l'API du chatflow que lorsque l'en-tête Authorization est fourni avec la clé API correcte spécifiée lors d'un appel HTTP.

```json
"Authorization": "Bearer <your-api-key>"
```

Un exemple d'appel de l'API en utilisant POSTMAN

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

