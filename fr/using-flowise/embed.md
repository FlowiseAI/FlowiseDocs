---
description: Learn how to customize and embed our chat widget
---

# Encombrer

***

Vous pouvez facilement ajouter le widget de chat à votre site Web. Copiez simplement le script de widget fourni et collez-le entre le`<body>`et`</body>`Tags de votre fichier HTML.

<gigne> <img src = "../. Gitbook / Assets / image (8) (2) (1) (1) .png" alt = ""> <figcaption> </gigcaption> </gigust>

## Configuration du widget

La vidéo suivante montre comment injecter le script de widget dans n'importe quelle page Web.

{% embed url = "https://github.com/flowiseai/flowise/assets/26460777/c128829a-2d08-4d60-b821-1e41a9e677d0"%}

## En utilisant une version spécifique

Vous pouvez spécifier la version de Flowise-Embed's`web.js`à utiliser. Pour la liste complète des versions:[https://www.npmjs.com/package/flowise-embed](https://www.npmjs.com/package/flowise-embed)

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed@<some-version>/dist/web.js';
  Chatbot.init({
    chatflowid: 'your-chatflowid-here',
    apiHost: 'your-apihost-here',
  })
</script>
```

{% hint style = "avertissement"%}
Dans Flowise ** v2.1.0 **, nous avons modifié le fonctionnement du streaming. Si votre version Flowise est inférieure à cela, vous pouvez trouver votre chatbot intégré à recevoir des messages.

Vous pouvez soit mettre à jour le flux vers ** v2.1.0 ** et au-dessus

Ou, si pour une raison quelconque, vous préférez ne pas mettre à jour Flowise, vous pouvez spécifier la dernière version ** v1.x.x ** de[Flowise-Embed](https://www.npmjs.com/package/flowise-embed?activeTab=versions). Dernière maintenue`web.js`La version est ** v1.3.14. **

Par exemple:

`https://cdn.jsdelivr.net/npm/flowise-embed@1.3.14/dist/web.js`
{% EndHint%}

## ConfigFlow Config

Tu peux passer`chatflowConfig`Objet JSON pour remplacer la configuration existante. C'est la même chose que[Broken link](broken-reference "mention")en API.

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
  Chatbot.init({
    chatflowid: 'your-chatflowid-here',
    apiHost: 'your-apihost-here',
    chatflowConfig: {
      "sessionId": "123",
      "returnSourceDocuments": true
    }
  })
</script>
```

## Configuration d'observateur

Cela vous permet d'exécuter du code dans le parent en fonction des observations de signal dans le chatbot.

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
  Chatbot.init({
    chatflowid: 'your-chatflowid-here',
    apiHost: 'your-apihost-here',
    observersConfig: {
      // User input has changed
      observeUserInput: (userInput) => {
        console.log({ userInput });
      },
      // The bot message stack has changed
      observeMessages: (messages) => {
        console.log({ messages });
      },
      // The bot loading signal changed
      observeLoading: (loading) => {
        console.log({ loading });
      },
    },
  })
</script>
```

## Thème

Vous pouvez modifier l'apparence complète du chatbot intégré et activer les fonctionnalités telles que les infractions, les avertissements, les messages de bienvenue personnalisés, et plus en utilisant la propriété du thème. Cela vous permet de personnaliser profondément l'apparence du widget, notamment:

* ** Bouton: ** Position, taille, couleur, icône, comportement de glisser-déposer et d'ouverture automatique.
* ** Infiltration: ** Visibilité, texte du message, couleur d'arrière-plan, couleur du texte et taille de police.
* ** Avis de non-responsabilité: ** Titre, message, couleurs pour le texte, les boutons et l'arrière-plan, y compris une option de superposition floue.
* ** Fenêtre de chat: ** Titre, agent / affichage des messages utilisateur, messages de bienvenue / d'erreur, couleur / image d'arrière-plan, dimensions, taille de police, invites de démarrage, rendu HTML, style de message (couleurs, avatars), comportement d'entrée de texte (Couleurs, limites de caractère, sons), options de rétroaction, affichage de date / temps et personnalisation de la page.
* ** CSSS personnalisé: ** Injectez directement le code CSS pour un contrôle encore plus fin sur l'apparence, remplacement des styles par défaut au besoin ([see the instructions guide below](embed.md#custom-css-modification))

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
  Chatbot.init({
    chatflowid: 'your-chatflowid-here',
    apiHost: 'your-apihost-here',
    theme: {
      button: {
        backgroundColor: '#3B81F6',
        right: 20,
        bottom: 20,
        size: 48, // small | medium | large | number
        dragAndDrop: true,
        iconColor: 'white',
        customIconSrc: 'https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/svg/google-messages.svg',
        autoWindowOpen: {
          autoOpen: true, //parameter to control automatic window opening
          openDelay: 2, // Optional parameter for delay time in seconds
          autoOpenOnMobile: false, //parameter to control automatic window opening in mobile
        },
      },
      tooltip: {
        showTooltip: true,
        tooltipMessage: 'Hi There 👋!',
        tooltipBackgroundColor: 'black',
        tooltipTextColor: 'white',
        tooltipFontSize: 16,
      },
      disclaimer: {
        title: 'Disclaimer',
        message: 'By using this chatbot, you agree to the <a target="_blank" href="https://flowiseai.com/terms">Terms & Condition</a>',
        textColor: 'black',
        buttonColor: '#3b82f6',
        buttonText: 'Start Chatting',
        buttonTextColor: 'white',
        blurredBackgroundColor: 'rgba(0, 0, 0, 0.4)', //The color of the blurred background that overlays the chat interface
        backgroundColor: 'white',
      },
      customCSS: ``, // Add custom CSS styles. Use !important to override default styles
      chatWindow: {
        showTitle: true,
        showAgentMessages: true,
        title: 'Flowise Bot',
        titleAvatarSrc: 'https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/svg/google-messages.svg',
        titleTextColor: '#ffffff',
        titleBackgroundColor: '#3B81F6',
        welcomeMessage: 'Hello! This is custom welcome message',
        errorMessage: 'This is a custom error message',
        backgroundColor: '#ffffff',
        backgroundImage: 'enter image path or link', // If set, this will overlap the background color of the chat window.
        height: 700,
        width: 400,
        fontSize: 16,
        starterPrompts: ['What is a bot?', 'Who are you?'], // It overrides the starter prompts set by the chat flow passed
        starterPromptFontSize: 15,
        clearChatOnReload: false, // If set to true, the chat will be cleared when the page reloads
        sourceDocsTitle: 'Sources:',
        renderHTML: true,
        botMessage: {
          backgroundColor: '#f7f8ff',
          textColor: '#303235',
          showAvatar: true,
          avatarSrc: 'https://raw.githubusercontent.com/zahidkhawaja/langchain-chat-nextjs/main/public/parroticon.png',
        },
        userMessage: {
          backgroundColor: '#3B81F6',
          textColor: '#ffffff',
          showAvatar: true,
          avatarSrc: 'https://raw.githubusercontent.com/zahidkhawaja/langchain-chat-nextjs/main/public/usericon.png',
        },
        textInput: {
          placeholder: 'Type your question',
          backgroundColor: '#ffffff',
          textColor: '#303235',
          sendButtonColor: '#3B81F6',
          maxChars: 50,
          maxCharsWarningMessage: 'You exceeded the characters limit. Please input less than 50 characters.',
          autoFocus: true, // If not used, autofocus is disabled on mobile and enabled on desktop. true enables it on both, false disables it on both.
          sendMessageSound: true,
          // sendSoundLocation: "send_message.mp3", // If this is not used, the default sound effect will be played if sendSoundMessage is true.
          receiveMessageSound: true,
          // receiveSoundLocation: "receive_message.mp3", // If this is not used, the default sound effect will be played if receiveSoundMessage is true.
        },
        feedback: {
          color: '#303235',
        },
        dateTimeToggle: {
          date: true,
          time: true,
        },
        footer: {
          textColor: '#303235',
          text: 'Powered by',
          company: 'Flowise',
          companyLink: 'https://flowiseai.com',
        },
      },
    },
  });
</script>
```

** Remarque: ** Voir plein[configuration list](https://github.com/FlowiseAI/FlowiseChatEmbed#configuration)

## Modification du code personnalisé

Pour modifier le code source complet du widget de chat embarqué, suivez ces étapes:

1. Fourchez le[Flowise Chat Embed](https://github.com/FlowiseAI/FlowiseChatEmbed)dépôt
2. Courir`yarn install`Pour installer les dépendances nécessaires
3. Ensuite, vous pouvez apporter des modifications au code
4. Courir`yarn build`Pour récupérer les changements
5. Poussez les modifications au référentiel fourchu
6. Vous pouvez ensuite utiliser votre personnalité`web.js`comme un chat intégré comme ça:

Remplacer`username`à votre nom d'utilisateur GitHub, et`forked-repo`à votre dépôt fourchu.

<pre class = "Language-html"> <code class = "lang-html"> <strong> <script type = "module">
</strong> Importer le chatbot depuis "https://cdn.jsdelivr.net/gh/username/forked-repo/dist/web.js"
Chatbot.init ({
Chatflowid: "Votre chatflowid-here",
apihost: "votre-apihost-here",
      })
</cript>
</code> </pre>

<gigne> <img src = "../. GitBook / Assets / image (1) (1) (2) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

```html
<script type="module">
      import Chatbot from "https://cdn.jsdelivr.net/gh/HenryHengZJ/FlowiseChatEmbed-Test/dist/web.js"
      Chatbot.init({
          chatflowid: "your-chatflowid-here",
          apiHost: "your-apihost-here",
      })
</script>
```

{% hint style = "info"%}
Une alternative à jsdelivr est un peu. Voici un exemple:

<pre> <code> <strong> https://unpkg.com/flowise-embed/dist/web.js
</strong> </code> </pre>
{% EndHint%}

## Modification CSS personnalisée

Vous pouvez désormais ajouter directement des CSS personnalisés pour styliser votre widget de chat intégré, éliminant le besoin de personnalité`web.js`Fichiers (nécessite v2.0.8 ou version ultérieure). Cela vous permet de:

* Donnez à chaque chatbot intégré un aspect et une sensation uniques
* Utilisez le fonctionnaire`web.js`—Les versions ou hébergeurs plus personnalisés ne sont nécessaires pour le style
* Mettre à jour les styles instantanément

Voici comment l'utiliser:

```html
<script src="https://cdn.jsdelivr.net/gh/FlowiseAI/FlowiseChatEmbed@main/dist/web.js"></script>
<script>
  Chatbot.init({
    chatflowid: "your-chatflowid-here",
    apiHost: "your-apihost-here",
    theme: {
      // ... other theme settings
      customCSS: `
        /* Your custom CSS here */
        /* Use !important to override default styles */
      `,
    }
  });
</script>
```

## Cors

Lorsque vous utilisez un widget de chat intégré, il y a une chance que vous puissiez faire face à un problème COR comme:

{% hint style = "danger"%}
L'accès à la récupération à 'https: // \ <your-flowise.com> / api / v1 / prédiction /' From Origin 'https: // \ <your-flowise.com>' n'a été bloqué par la politique CORS: pas de «Access-Control-Allow-origin» Header n'est présent sur la ressource demandée.
{% EndHint%}

Pour le réparer, spécifiez les variables d'environnement suivantes:

```
CORS_ORIGINS=*
IFRAME_ORIGINS=*
```

Par exemple, si vous utilisez`npx flowise start`

```
npx flowise start --CORS_ORIGINS=* --IFRAME_ORIGINS=*
```

Si vous utilisez Docker, placez les variables Env à l'intérieur`Flowise/docker/.env`

Si vous utilisez un clone Git local, placez les variables Env à l'intérieur`Flowise/packages/server/.env`

## Tutoriels vidéo

Ces deux vidéos vous apprendront à intégrer le widget Flowise dans un site Web.

{% embed url = "https://youtu.be/4paq2wobdq4"%}

{% embed url = "https://youtu.be/xoecv1xyn48"%}
