---
description: Learn how to analyze and troubleshoot your chatflows and agentflows
---

# Analytic

***

There are several analytic providers Flowise integrates with:

* [LunaryAI](https://lunary.ai/)
* [Langsmith](https://smith.langchain.com/)
* [Langfuse](https://langfuse.com/)
* [LangWatch](https://langwatch.ai/)
* [Arize](https://arize.com/)
* [Phoenix](https://phoenix.arize.com/)
* [Opik](https://www.comet.com/site/products/opik/)

## Lunary

[Lunary](https://lunary.ai/) is a monitoring and analytics platform for LLM chatbots.

Flowise has partnered with Lunary to provide a complete integration supporting user tracing, feedback tracking, conversation replays and detailed LLM analytics.

Flowise users can get a 30% discount on the Teams Plan using code `FLOWISEFRIENDS` during checkout.

Read more on how to setup Lunary with Flowise [here](https://lunary.ai/docs/integrations/flowise).

## Setup

1. At the top right corner of your Chatflow or Agentflow, click **Settings** > **Configuration**

<figure><img src="../../.gitbook/assets/analytic-1.webp" alt="Screenshot of user clicking in the configuration menu" width="375"><figcaption></figcaption></figure>

2. Then go to the Analyse Chatflow section

<figure><img src="../../.gitbook/assets/analytic-2.png" alt="Screenshot of the Analyse Chatflow section with the different Analytics providers"><figcaption></figcaption></figure>

3. You will see a list of providers, along with their configuration fields

<figure><img src="../../.gitbook/assets/image (82).png" alt="Screenshot of an analytics provider with credentials fields expanded"><figcaption></figcaption></figure>

4. Fill in the credentials and other configuration details, then turn the provider **ON**. Click Save.

<figure><img src="../../.gitbook/assets/image (83).png" alt="Screenshot of analytics providers enabled"><figcaption></figcaption></figure>

## API

Once the analytic has been turned ON from the UI, you can override or provide additional configuration in the body of the [Prediction API](api.md#prediction-api):

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
