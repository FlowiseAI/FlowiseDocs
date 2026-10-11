---
description: 了解如何自定义和嵌入我们的聊天小部件
---

# 嵌入

***

您可以轻松地将聊天小部件添加到您的网站。只需复制提供的小部件脚本并将其粘贴到 `<body>` and `</body>` HTML 文件的标签。

<figure><img src="../.gitbook/assets/image (8) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 小部件设置

以下视频演示了如何将小部件脚本注入任何网页。

{% embed url="https://github.com/FlowiseAI/Flowise/assets/26460777/c128829a-2d08-4d60-b821-1e41a9e677d0" %}

## 使用特定版本

您可以指定要使用的 flowise-embed 的 `web.js` 版本。有关版本的完整列表：[https://www.npmjs.com/package/flowise-embed](https://www.npmjs.com/package/flowise-embed)

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
在 Flowise **v2.1.0** 中，我们修改了流式传输的工作方式。如果您的 Flowise 版本低于该版本，您可能会发现嵌入式聊天机器人无法接收消息。

您可以将 Flowise 更新到 **v2.1.0** 及更高版本

或者，如果由于某种原因您不想更新 Flowise，您可以指定 [Flowise-嵌入](https://www.npmjs.com/package/flowise-embed?activeTab=versions) 的最新 **v1.x.x** 版本。最后维护的 `web.js` 版本是 **v1.3.14.**

例如：

`https://cdn.jsdelivr.net/npm/flowise-embed@1.3.14/dist/web.js`
{% endhint %}

## 聊天流配置

您可以传递 `chatflowConfig` JSON 对象来覆盖现有配置。这与 API 中的[损坏的链接](broken-reference "mention")相同。

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

## 观察者配置

这允许您根据聊天机器人内的信号观察在父级中执行代码。

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

## 主题

您可以使用主题属性更改嵌入式聊天机器人的完整外观并启用工具提示词、免责声明、自定义欢迎消息等功能。这允许您深度自定义小部件的外观和感觉，包括：

* **按钮：** 位置、大小、颜色、图标、拖放行为和自动打开。
* **工具提示词：** 可见性、消息文本、背景颜色、文本颜色和字体大小。
* **免责声明：** 标题、消息、文本颜色、按钮和背景，包括模糊覆盖选项。
* **聊天窗口：** 标题、代理/用户消息显示、欢迎/错误消息、背景颜色/图像、尺寸、字体大小、启动提示词、HTML 渲染、消息样式（颜色、头像）、文本输入行为（占位符、颜色、字符限制、声音）、反馈选项、日期/时间显示和页脚自定义。
* **自定义 CSS：** 直接注入 CSS 代码，以更好地控制外观，根据需要覆盖默认样式（[请参阅下面的说明指南](embed.md#custom-css-modification)）

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

**注意：** 查看完整的[配置列表](https://github.com/FlowiseAI/FlowiseChatEmbed#configuration)

## 自定义代码修改

要修改嵌入式聊天小部件的完整源代码，请按照以下步骤操作：

1. 分叉 [Flowise Chat 嵌入](https://github.com/FlowiseAI/FlowiseChatEmbed) 存储库
2. 运行 `yarn install` 安装必要的依赖项
3.然后您可以进行任何代码更改
4. 运行 `yarn build` 以获取更改
5. 将更改推送到分叉存储库
6. 然后，您可以使用自定义 `web.js` 作为嵌入式聊天，如下所示：

将 `username` 替换为您的 Github 用户名，并将 `forked-repo` 替换为您的分叉存储库。

<pre class="language-html"><code class="lang-html"><strong>&#x3C;script type="module">
</strong>从“https://cdn.jsdelivr.net/gh/username/forked-repo/dist/web.js”导入聊天机器人
      聊天机器人.init({
          chatflowid: "您的chatflowid-这里",
          apiHost: "您的 apihost-这里",
      })
</脚本>
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
jsdelivr 的替代方案是 unpkg。这是一个例子：

<pre><code><strong>https://unpkg.com/flowise-embed/dist/web.js
</strong></code></pre>
{% endhint %}

## 自定义CSS修改

您现在可以直接添加自定义 CSS 来设计嵌入式聊天小部件的样式，从而无需自定义 `web.js` 文件（需要 v2.0.8 或更高版本）。这使您能够：

* 为每个嵌入式聊天机器人提供独特的外观和感觉
* 使用官方 `web.js` — 样式不再需要自定义构建或托管
* 即时更新样式

使用方法如下：

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

使用嵌入式聊天小部件时，您可能会遇到 CORS 问题，例如：

{% hint style="danger" %}
访问“https://\”处的获取<your-flowise.com>/api/v1/prediction/' 来自原点 'https://\<your-flowise.com>' 已被 CORS 策略阻止：请求的资源上不存在“Access-Control-Allow-Origin”标头。
{% endhint %}

要修复它，请指定以下环境变量：

```
CORS_ORIGINS=*
IFRAME_ORIGINS=*
```

例如，如果您使用 `npx flowise start`

```
npx flowise start --CORS_ORIGINS=* --IFRAME_ORIGINS=*
```

如果使用 Docker，请将环境变量放置在 `Flowise/docker/.env` 内

如果使用本地 Git 克隆，请将环境变量放在 `Flowise/packages/server/.env` 内

## 视频教程

这两个视频将教您如何将 Flowise 小部件嵌入到网站中。

{% embed url="https://youtu.be/4paQ2wObDQ4" %}

{% embed url="https://youtu.be/XOeCV1xyN48" %}
