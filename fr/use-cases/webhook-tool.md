---
description: Learn how to call a webhook on Make
---

# Appeler webhook

***

Ce tutoriel vous guide à travers la création d'un outil personnalisé dans Flowiseai qui appelle un point de terminaison WebHook, passant les paramètres nécessaires dans le corps de la demande. Nous utiliserons[Make.com](https://www.make.com/en)Pour configurer un flux de travail WebHook qui envoie des messages à un canal Discord.

## Configuration d'un webhook dans Make.com

1. Inscrivez-vous ou connectez-vous à[Make.com](https://www.make.com/en).
2. Créez un nouveau flux de travail contenant un module ** webhook ** et un module ** Discord **, comme indiqué ci-dessous:

<gigne> <img src = "../. GitBook / Assets / Screly-1691756705932.png" alt = "Exemple de workflow"> <Figcaption> </ Figcaption> </gigust>

3. À partir du module ** webhook **, copiez l'URL WebHook:

<gigne> <img src = "../. GitBook / Assets / image (46) .png" alt = "webhook url" width = "563"> <figCaption> </ Figcaption> </gistre>

4. Dans le module ** Discord **, configurez-le pour passer le`message`à partir du corps WebHook comme le message envoyé au canal Discord:

<gigne> <img src = "../. GitBook / Assets / Image (47) .png" alt = "Discord Module Configuration" width = "563"> <Figcaption> </ Figcaption> </gistre>

5. Cliquez sur ** Exécuter une fois ** pour commencer à écouter les demandes entrantes.
6. Envoyez une demande de poste de test avec le corps JSON suivant:

   ```json
   {
       "message": "Hello Discord!"
   }
   ```

<gigne> <img src = "../. GitBook / Assets / image (48) .png" alt = "Envoi de la demande de post" width = "563"> <Figcaption> </gigcaption> </gigust>

7. En cas de succès, vous verrez le message apparaître dans votre canal Discord:

<Figure> <img src = "../. GitBook / Assets / Image (49) .png" alt = "Discord Message" width = "249"> <Figcaption> </gigcaption> </ Figure>

Félicitations! Vous avez réussi à configurer un flux de travail WebHook qui envoie des messages à Discord. 🎉

## Création d'un outil Webhook dans Flowiseai

Ensuite, nous créerons un outil personnalisé dans FlowiSeai pour envoyer des demandes WebHook.

### Étape 1: Ajouter un nouvel outil

1. Ouvrez le tableau de bord ** Flowiseai **.
2. Cliquez sur ** Outils **, puis sélectionnez ** Créer **.

<gigne> <img src = "../. GitBook / Assets / Screly-1691758397783.png" alt = "Création de l'outil dans FlowiSeai"> <Figcaption> </gigcaption> </gistre>

3. Remplissez les champs suivants:

| Champ | Valeur |
   |-------|-------|
| ** Nom de l'outil ** |`make_webhook`(doit être dans Snake_case) |
| ** Description de l'outil ** | Utile lorsque vous devez envoyer des messages à Discord |
| ** Icône d'outil Src ** |[Flowise Tool Icon](https://github.com/FlowiseAI/Flowise/assets/26460777/517fdab2-8a6e-4781-b3c8-fb92cc78aa0b) |

4. Définissez le schéma de saisie ** **:

<gigne> <img src = "../. GitBook / Assets / Image (167) .png" alt = "Exemple de schéma de saisie"> <figcaption> </figcaption> </gigust>

### Étape 2: Ajouter la logique de demande de webhook

Entrez la fonction JavaScript suivante:

```javascript
const fetch = require('node-fetch');
const webhookUrl = 'https://hook.eu1.make.com/abcdef';
const body = {
    "message": $message
};
const options = {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(body)
};
try {
    const response = await fetch(webhookUrl, options);
    const text = await response.text();
    return text;
} catch (error) {
    console.error(error);
    return '';
}
```

5. Cliquez sur ** Ajouter ** pour enregistrer votre outil personnalisé.

<gigne> <img src = "../. GitBook / Assets / Image (51) .png" alt = "Tool Ajout Confirmation" width = "279"> <Figcaption> </gigcaption> </gigust>

### Étape 3: Créez un ChatFlow avec l'intégration de webhook

1. Créez une nouvelle toile et ajoutez les nœuds suivants:
   - ** Mémoire de tampon **
   - ** Chatopenai **
   - ** Outil personnalisé ** (Sélectionner`make_webhook`)
   - ** Agent de fonction Openai **

2. Connectez-les comme indiqué:

<gigne> <img src = "../. GitBook / Assets / Screly-1691758990676.png" alt = "ChatFlow Configuration"> <Figcaption> </gigcaption> </gistre>

3. Enregistrez le ChatFlow et commencez à le tester.

### Étape 4: Envoi de messages via webook

Essayez de poser au chatbot une question comme:

> _ "Comment faire cuire un œuf?" _

Ensuite, demandez à l'agent d'envoyer ces informations à Discord:

<gigne> <img src = "../. GitBook / Assets / image (53) .png" alt = "Envoi du message via l'agent" width = "563"> <Figcaption> </ / Figcaption> </ Figure>

Vous devriez voir le message apparaître dans votre canal Discord:

<gigne> <img src = "../. GitBook / Assets / Image (54) .png" alt = "Message final dans Discord"> <FigCaption> </Figcaption> </gigne>

### Outils de test de webhook alternatifs

Si vous souhaitez tester les webhooks sans Make.com, envisagez d'utiliser:

- [Beeceptor](https://beeceptor.com)- Configurez rapidement un point de terminaison API simulé.
- [Webhook.site](https://webhook.site)- Inspectez et déboguez les demandes HTTP en temps réel.
- [Pipedream RequestBin](https://pipedream.com/requestbin)- Capturez et analysez les webhooks entrants.

## Plus de tutoriels

- Regardez un guide étape par étape sur l'utilisation de webhooks avec des outils personnalisés Flowise:
{% embed url = "https://youtu.be/_k9xjqegnru"%}

- Apprenez à connecter Flowise aux feuilles Google à l'aide de webhooks:
{% embed url = "https://youtu.be/fehxlDrljfo"%}

- Apprenez à connecter Flowise à Microsoft Excel à l'aide de webhooks:
{% embed url = "https://youtu.be/cb2gc8jznjc"%}

En suivant ce guide, vous pouvez déclencher dynamiquement les workhook workflows et étendre l'automatisation à divers services comme Gmail, Google Sheets, etc.
