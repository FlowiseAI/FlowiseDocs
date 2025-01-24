---
description: Learn how to customize and embed our chat widget
---

# Embed

***

You can easily add the chat widget to your website. Just copy the provided widget script and paste it anywhere between the `<body>` and  `</body>`  tags of your HTML file.

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

## Configuración

You can customize the widget with the following options:

```javascript
Chatbot.init({
    chatflowid: "your-chatflowid-here",
    apiHost: "http://localhost:3000",
    chatflowConfig: {
        // Sobrescribe la configuración del flujo de chat
        systemMessagePrompt: "You are a helpful AI assistant",
        temperature: 0,
        // ... otras configuraciones
    },
    theme: {
        button: {
            backgroundColor: "#3B81F6",
            right: 20,
            bottom: 20,
            size: "medium",
            iconColor: "white",
            customIconSrc: "https://raw.githubusercontent.com/walkxcode/dashboard-icons/main/svg/google-messages.svg",
            // Opciones de tamaño: "small" | "medium" | "large"
        },
        chatWindow: {
            welcomeMessage: "¡Hola! ¿En qué puedo ayudarte?",
            backgroundColor: "#ffffff",
            height: 700,
            width: 400,
            fontSize: 16,
            poweredByTextColor: "#303235",
            botMessage: {
                backgroundColor: "#f7f8ff",
                textColor: "#303235",
                showAvatar: true,
                avatarSrc: "https://raw.githubusercontent.com/zahidkhawaja/langchain-chat-nextjs/main/public/parroticon.png",
            },
            userMessage: {
                backgroundColor: "#3B81F6",
                textColor: "#ffffff",
                showAvatar: true,
                avatarSrc: "https://raw.githubusercontent.com/zahidkhawaja/langchain-chat-nextjs/main/public/usericon.png",
            },
            textInput: {
                placeholder: "Escribe tu pregunta",
                backgroundColor: "#ffffff",
                textColor: "#303235",
                sendButtonColor: "#3B81F6",
                maxChars: 50,
                maxCharsWarningMessage: "Has excedido el límite de caracteres. Por favor, ingresa menos de 50 caracteres.",
                autoFocus: true, // Si no se usa, el autofocus está deshabilitado en móvil y habilitado en escritorio. true lo habilita en ambos, false lo deshabilita en ambos.
                sendMessageSound: true,
                // sendSoundLocation: "send_message.mp3", // Si no se usa, se reproducirá el efecto de sonido por defecto si sendSoundMessage es true.
                receiveMessageSound: true,
            }
        }
    }
})
```

## Variables de Entorno

To enable the widget on your website, you need to configure the following environment variables:

```properties
CORS_ORIGINS="http://localhost:3000,http://example.com"
```

If using Docker, place the env variables inside `Flowise/docker/.env`

If using local Git clone, place the env variables inside `Flowise/packages/server/.env`

## Video Tutorials

These two videos will teach you how to embed the Flowise widget into a website.

{% embed url="https://youtu.be/4paQ2wObDQ4" %}

{% embed url="https://youtu.be/XOeCV1xyN48" %}
