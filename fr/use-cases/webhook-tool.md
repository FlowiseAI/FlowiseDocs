---
description: Learn how to call a webhook on Make
---

# Calling Webhook

***

This tutorial walks you through creating a custom tool in FlowiseAI that calls a webhook endpoint, passing the necessary parameters in the request body. We will use [Make.com](https://www.make.com/en) to set up a webhook workflow that sends messages to a Discord channel.

## Setting Up a Webhook in Make.com

1. Sign up or log in to [Make.com](https://www.make.com/en).
2. Create a new workflow containing a **Webhook** module and a **Discord** module, as shown below:

   <figure><img src="../.gitbook/assets/screely-1691756705932.png" alt="Workflow example"><figcaption></figcaption></figure>

3. From the **Webhook** module, copy the webhook URL:

   <figure><img src="../.gitbook/assets/image (46).png" alt="Webhook URL" width="563"><figcaption></figcaption></figure>

4. In the **Discord** module, configure it to pass the `message` from the webhook body as the message sent to the Discord channel:

   <figure><img src="../.gitbook/assets/image (47).png" alt="Discord module setup" width="563"><figcaption></figcaption></figure>

5. Click **Run Once** to start listening for incoming requests.
6. Send a test POST request with the following JSON body:

   ```json
   {
       "message": "Hello Discord!"
   }
   ```

   <figure><img src="../.gitbook/assets/image (48).png" alt="Sending POST request" width="563"><figcaption></figcaption></figure>

7. If successful, you will see the message appear in your Discord channel:

   <figure><img src="../.gitbook/assets/image (49).png" alt="Discord message" width="249"><figcaption></figcaption></figure>

Congratulations! You have successfully set up a webhook workflow that sends messages to Discord. 🎉

## Creating a Webhook Tool in FlowiseAI

Next, we will create a custom tool in FlowiseAI to send webhook requests.

### Step 1: Add a New Tool

1. Open the **FlowiseAI** dashboard.
2. Click **Tools**, then select **Create**.

   <figure><img src="../.gitbook/assets/screely-1691758397783.png" alt="Creating tool in FlowiseAI"><figcaption></figcaption></figure>

3. Fill in the following fields:

   | Field | Value |
   |-------|-------|
   | **Tool Name** | `make_webhook` (must be in snake_case) |
   | **Tool Description** | Useful when you need to send messages to Discord |
   | **Tool Icon Src** | [Flowise Tool Icon](https://github.com/FlowiseAI/Flowise/assets/26460777/517fdab2-8a6e-4781-b3c8-fb92cc78aa0b) |

4. Define the **Input Schema**:

   <figure><img src="../.gitbook/assets/image (167).png" alt="Input schema example"><figcaption></figcaption></figure>

### Step 2: Add Webhook Request Logic

Enter the following JavaScript function:

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

5. Click **Add** to save your custom tool.

   <figure><img src="../.gitbook/assets/image (51).png" alt="Tool added confirmation" width="279"><figcaption></figcaption></figure>

### Step 3: Build a Chatflow with Webhook Integration

1. Create a new canvas and add the following nodes:
   - **Buffer Memory**
   - **ChatOpenAI**
   - **Custom Tool** (select `make_webhook`)
   - **OpenAI Function Agent**

2. Connect them as shown:

   <figure><img src="../.gitbook/assets/screely-1691758990676.png" alt="Chatflow setup"><figcaption></figcaption></figure>

3. Save the chatflow and start testing it.

### Step 4: Sending Messages via Webhook

Try asking the chatbot a question like:

> _"How to cook an egg?"_

Then, request the agent to send this information to Discord:

   <figure><img src="../.gitbook/assets/image (53).png" alt="Sending message via agent" width="563"><figcaption></figcaption></figure>

You should see the message appear in your Discord channel:

   <figure><img src="../.gitbook/assets/image (54).png" alt="Final message in Discord"><figcaption></figcaption></figure>

### Alternative Webhook Testing Tools

If you want to test webhooks without Make.com, consider using:

- [Beeceptor](https://beeceptor.com) – Quickly set up a mock API endpoint.
- [Webhook.site](https://webhook.site) – Inspect and debug HTTP requests in real-time.
- [Pipedream RequestBin](https://pipedream.com/requestbin) – Capture and analyze incoming webhooks.

## More Tutorials

- Watch a step-by-step guide on using webhooks with Flowise custom tools:
  {% embed url="https://youtu.be/_K9xJqEgnrU" %}

- Learn how to connect Flowise to Google Sheets using webhooks:
  {% embed url="https://youtu.be/fehXLdRLJFo" %}

- Learn how to connect Flowise to Microsoft Excel using webhooks:
  {% embed url="https://youtu.be/cB2GC8JznJc" %}

By following this guide, you can trigger webhook workflows dynamically and extend automation to various services like Gmail, Google Sheets, and more.
