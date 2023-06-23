# Zep Memory

[Zep](https://github.com/getzep/zep) is long-term memory store for LLM applications. It stores, summarizes, embeds, indexes, and enriches LLM app / chatbot histories, and exposes them via simple, low-latency APIs.

It is suitable when you are using Flowise as API, and want the AI to remember conversations between different users. You can achieve that using **sessionId**.

## Deploy Zep

You can easily deploy Zep to cloud services like [Render](https://render.com/), [Flyio](https://fly.io/). If you prefer to test it locally, you can also spin up a docker container by following their [quick guide](https://github.com/getzep/zep#quick-start).

In this example, we are going to deploy to Render.

1. Head over to [Zep Repo](https://github.com/getzep/zep#quick-start) and click **Deploy to Render**
2. This will bring you to Render's Blueprint page and simply click **Create New Resources**

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

3. When the deployment is done, you should see 3 applications created on your dashboard

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

4. Simply click the first one called **zep** and copy the deployed URL

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

## Use in Flowise UI

1. Back to Flowise application, simply create a new canvas or use one of the template from marketplace. In this example, we are going to use **Simple Conversational Chain**

<figure><img src="../.gitbook/assets/Untitled (3).png" alt=""><figcaption></figcaption></figure>

2. Replace **Buffer Memory** with **Zep Memory**. Then replace the **Base URL** with the Zep URL you have copied above

<figure><img src="../.gitbook/assets/Untitled.png" alt=""><figcaption></figcaption></figure>

3. Save the chatflow and test it out to see if conversations are remembered.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

4. Now try clearing the chat history, and see if it still remember the previous conversations

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

By default, the first chat message id will be used as **sessionId** for Zep Memory, allowing AI to remember the conversations.

When chat history is cleared, all chat messages will be deleted. When user ask a new question, a new chat message is being saved with a new id. The id of that first new chat message will then be used as the new **sessionId**. As you can see from picture above, it does not remember user's name after the chat history is cleared.

## Use in Flowise API

To use as API, especially when multiple users are interacting with the flow, you can use **sessionId** to differentiate conversations between each user.

<figure><img src="../.gitbook/assets/Untitled (1).png" alt=""><figcaption></figcaption></figure>

For instance, in your front-end application, you can dynamically generate a **sessionId** using a combination of **userid** and **timestamp**. When user clear the chats from your front-end application, you can generate a new **sessionId** with the same **userid** and <mark style="color:red;">**new**</mark> **timestamp**. This will begins a new chat session with no previous conversations memory.

Example of a POST call:

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>
