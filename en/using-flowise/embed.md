---
description: Learn how to customize and embed our chat widget
---

# Embed

***

You can easily add the chat widget to your website. Just copy the provided widget script and paste it anywhere between the `<body>` and `</body>` tags of your HTML file.

<figure><img src="../.gitbook/assets/image (8) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Widget Setup

The following video shows how to inject the widget script into any webpage.

{% embed url="https://github.com/FlowiseAI/Flowise/assets/26460777/c128829a-2d08-4d60-b821-1e41a9e677d0" %}

## Using Specific Version

You can specify the version of flowise-embed's `web.js` to use. For full list of versions: [https://www.npmjs.com/package/flowise-embed](https://www.npmjs.com/package/flowise-embed)

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed@<some-version>/dist/web.js';
  Chatbot.init({
    chatflowid: 'your-chatflowid-here',
    apiHost: 'your-apihost-here',
  })
</script>
```

{% hint style="warning" %}
In Flowise **v2.1.0**, we have modified the way streaming works. If your Flowise version is lower than that, you might find your embedded chatbot not able to receive messages.

You can either update Flowise to **v2.1.0** and above

Or, if for some reason you prefer not to update Flowise, you can specify the latest **v1.x.x** version of [Flowise-Embed](https://www.npmjs.com/package/flowise-embed?activeTab=versions). Last maintained `web.js` version is **v1.3.14.**

For instance:

`https://cdn.jsdelivr.net/npm/flowise-embed@1.3.14/dist/web.js`
{% endhint %}

## Chatflow Config

You can pass `chatflowConfig` JSON object to override existing configuration. This is the same as [Broken link](broken-reference "mention") in API.

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

## Observer Config

This allows you to execute code in parent based upon signal observations within the chatbot.

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

## Theme

You can change the full appearance of the embedded chatbot and enable functionalities like tooltips, disclaimers, custom welcome messages, and more using the theme property. This allows you to deeply customize the look and feel of the widget, including:

* **Button:** Position, size, color, icon, drag-and-drop behavior, and automatic opening.
* **Tooltip:** Visibility, message text, background color, text color, and font size.
* **Disclaimer:** Title, message, colors for text, buttons, and background, including a blurred overlay option.
* **Chat Window:** Title, agent/user message display, welcome/error messages, background color/image, dimensions, font size, starter prompts, HTML rendering, message styling (colors, avatars), text input behavior (placeholder, colors, character limits, sounds), feedback options, date/time display, and footer customization.
* **Custom CSS:** Directly inject CSS code for even finer control over the appearance, overriding default styles as needed ([see the instructions guide below](embed.md#custom-css-modification))

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

**Note:** See full [configuration list](https://github.com/FlowiseAI/FlowiseChatEmbed#configuration)

## Custom Code Modification

To modify the full source code of embedded chat widget, follow these steps:

1. Fork the [Flowise Chat Embed](https://github.com/FlowiseAI/FlowiseChatEmbed) repository
2. Run `yarn install` to install the necessary dependencies
3. Then you can make any code changes
4. Run `yarn build` to pick up the changes
5. Push changes to the forked repository
6. You can then use your custom `web.js` as embedded chat like so:

Replace `username` to your Github username, and `forked-repo` to your forked repo.

<pre class="language-html"><code class="lang-html"><strong>&#x3C;script type="module">
</strong>      import Chatbot from "https://cdn.jsdelivr.net/gh/username/forked-repo/dist/web.js"
      Chatbot.init({
          chatflowid: "your-chatflowid-here",
          apiHost: "your-apihost-here",
      })
&#x3C;/script>
</code></pre>

<figure><img src="../.gitbook/assets/image (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

```html
<script type="module">
      import Chatbot from "https://cdn.jsdelivr.net/gh/HenryHengZJ/FlowiseChatEmbed-Test/dist/web.js"
      Chatbot.init({
          chatflowid: "your-chatflowid-here",
          apiHost: "your-apihost-here",
      })
</script>
```

{% hint style="info" %}
An alternative to jsdelivr is unpkg. Here is an example:

<pre><code><strong>https://unpkg.com/flowise-embed/dist/web.js
</strong></code></pre>
{% endhint %}

## Custom CSS Modification

You can now directly add custom CSS to style your embedded chat widget, eliminating the need for custom `web.js` files (requires v2.0.8 or later). This allows you to:

* Give each embedded chatbot a unique look and feel
* Use the official `web.js`—no more custom builds or hosting are needed for styling
* Update styles instantly

Here's how to use it:

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

## CORS

When using embedded chat widget, there's chance that you might face CORS issue like:

{% hint style="danger" %}
Access to fetch at 'https://\<your-flowise.com>/api/v1/prediction/' from origin 'https://\<your-flowise.com>' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
{% endhint %}

To fix it, specify the following environment variables:

```
CORS_ORIGINS=*
IFRAME_ORIGINS=*
```

For example, if you are using `npx flowise start`

```
npx flowise start --CORS_ORIGINS=* --IFRAME_ORIGINS=*
```

If using Docker, place the env variables inside `Flowise/docker/.env`

If using local Git clone, place the env variables inside `Flowise/packages/server/.env`

## Video Tutorials

These two videos will teach you how to embed the Flowise widget into a website.

{% embed url="https://youtu.be/4paQ2wObDQ4" %}

{% embed url="https://youtu.be/XOeCV1xyN48" %}
