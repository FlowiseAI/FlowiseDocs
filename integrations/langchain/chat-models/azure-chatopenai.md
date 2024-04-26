# Azure ChatOpenAI

## Prerequisite

1. [Log in](https://portal.azure.com/) or [sign up](https://azure.microsoft.com/en-us/free/) to Azure
2. [Create](https://portal.azure.com/#create/Microsoft.CognitiveServicesOpenAI) your Azure OpenAI and wait for approval approximately 10 business days
3. Your API key will be available at **Azure OpenAI** > click **name\_azure\_openai** > click **Click here to manage keys**

<figure><img src="../../../.gitbook/assets/azure/azure-general/1.png" alt=""><figcaption></figcaption></figure>

## Setup

### Azure ChatOpenAI

1. Click **Go to Azure OpenaAI Studio**

<figure><img src="../../../.gitbook/assets/azure/azure-general/2.png" alt=""><figcaption></figcaption></figure>

2. Click **Deployments**

<figure><img src="../../../.gitbook/assets/azure/azure-general/3.png" alt=""><figcaption></figcaption></figure>

3. Click **Create new deployment**

<figure><img src="../../../.gitbook/assets/azure/azure-general/4.png" alt=""><figcaption></figcaption></figure>

4. Select as shown below and click **Create**

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/1.png" alt="" width="558"><figcaption></figcaption></figure>

5. Successfully created **Azure ChatOpenAI**

* Deployment name: `gpt-35-turbo`
* Instance name: `top right conner`

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/2.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/azure/azure-general/2.png" alt=""><figcaption></figcaption></figure>

### Flowise

1. **Chat Models** > drag **Azure ChatOpenAI** node

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/3.png" alt="" width="563"><figcaption></figcaption></figure>

2. **Connect Credential** > click **Create New**

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/4.png" alt="" width="421"><figcaption></figcaption></figure>

3. Copy & Paste each details (API Key, Instance & Deployment name, [API Version](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference#chat-completions)) into **Azure ChatOpenAI** credential

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/5.png" alt="" width="563"><figcaption></figcaption></figure>

4. Voila [🎉](https://emojipedia.org/party-popper/), you have created **Azure ChatOpenAI node** in Flowise

<figure><img src="../../../.gitbook/assets/azure/azure-general/5.png" alt=""><figcaption></figcaption></figure>

## Resources

* [LangChain JS Azure ChatOpenAI](https://js.langchain.com/docs/modules/model\_io/models/chat/integrations/azure)
* [Azure OpenAI Service REST API reference](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference)
