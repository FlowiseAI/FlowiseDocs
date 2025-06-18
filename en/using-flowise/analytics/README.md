---
description: Learn how to analyze and troubleshoot your chatflows and agentflows
---

# Analytic

***

Flowise provides step by step tracing for [Agentflow V2](../agentflowv2.md):

<figure><img src="../../.gitbook/assets/image (332).png" alt=""><figcaption></figcaption></figure>

Besides, there are also several analytic providers Flowise integrates with:

* [LunaryAI](https://lunary.ai/)
* [Langsmith](https://smith.langchain.com/)
* [Langfuse](https://langfuse.com/)
* [LangWatch](https://langwatch.ai/)
* [Arize](https://arize.com/)
* [Phoenix](https://phoenix.arize.com/)
* [Opik](https://www.comet.com/site/products/opik/)

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
