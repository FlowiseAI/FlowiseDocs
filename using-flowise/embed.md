# Embed

You can embed a chat widget on your website. Simply copy/paste the embedded code provided by the embed button to anywhere in the `<body>` tag of your HTML file.

<figure><img src="../.gitbook/assets/image (8) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

Watch how to do that:

{% embed url="https://github.com/FlowiseAI/Flowise/assets/26460777/c128829a-2d08-4d60-b821-1e41a9e677d0" %}

You can also customize your own embedded chat widget UI. See the full [configuration list](https://github.com/FlowiseAI/FlowiseChatEmbed#configuration).

### Chatflow Config

You can pass a `chatflowConfig` JSON object to override existing configuration. This is the same as [#override-config](api.md#override-config "mention") in the API.

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
  Chatbot.init({
    chatflowid: 'abc',
    apiHost: 'http://localhost:3000',
    chatflowConfig: {
      "sessionId": "123",
      "returnSourceDocuments": true
    }
  })
</script>
```

### Observer Config

This allows you to execute code in the parent based upon signal observations within the chatbot.

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
  Chatbot.init({
    chatflowid: 'abc',
    apiHost: 'http://localhost:3000',
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

### Theme

You can change the pop-up button properties, as well as the chat window:

```html
<script type="module">
  import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
  Chatbot.init({
    chatflowid: 'abc',
    apiHost: 'http://localhost:3000',
    theme: {
      button: {
        backgroundColor: '#3B81F6',
        right: 20,
        bottom: 20,
        size: 'medium',
        iconColor: 'white',
        customIconSrc: 'https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/svg/google-messages.svg',
      },
      chatWindow: {
        showTitle: true, // show/hide the title bar
        title: 'Flowise Bot',
        titleAvatarSrc: 'https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/svg/google-messages.svg',
        welcomeMessage: 'Hello! This is custom welcome message',
        errorMessage: 'This is custom error message',
        backgroundColor: '#ffffff',
        height: 700,
        width: 400,
        fontSize: 16,
        poweredByTextColor: '#303235',
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
        },
      },
    },
  })
</script>
```

### Custom Modificaton

To modify the full source code of an embedded chat widget, follow these steps:

1. Fork the [Flowise Chat Embed](https://github.com/FlowiseAI/FlowiseChatEmbed) repository
2. Then you can make any code changes.  A popular change is to remove Flowise [branding](https://github.com/HenryHengZJ/FlowiseChatEmbed-Test/blob/main/src/components/Bot.tsx#L337).
3. Run `pnpm build`
4. Push changes to the forked repo
5. You can then use it as embedded chat like so:

Replace `username` with your Github username, and `forked-repo` with your forked repo.

<pre class="language-html"><code class="lang-html"><strong>&#x3C;script type="module">
</strong>      import Chatbot from "https://cdn.jsdelivr.net/gh/username/forked-repo/dist/web.js"
      Chatbot.init({
          chatflowid: "chatflow-id",
          apiHost: "http://localhost:3000",
      })
&#x3C;/script>
</code></pre>

<figure><img src="../.gitbook/assets/image (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

```html
<script type="module">
      import Chatbot from "https://cdn.jsdelivr.net/gh/HenryHengZJ/FlowiseChatEmbed-Test/dist/web.js"
      Chatbot.init({
          chatflowid: "chatflow-id",
          apiHost: "http://localhost:3000",
      })
</script>
```

{% hint style="info" %}
An alternative to jsdelivr is unpkg. For example:&#x20;

<pre><code><strong>https://unpkg.com/flowise-embed/dist/web.js
</strong></code></pre>
{% endhint %}

### Tutorials

* Watch how to embed Flowise in a Bootstrap 5 website

{% embed url="https://youtu.be/4paQ2wObDQ4" %}

* Watch how to add a chatbot to a website

{% embed url="https://youtu.be/XOeCV1xyN48" %}
