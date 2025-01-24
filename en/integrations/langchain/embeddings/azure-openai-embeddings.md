# Azure OpenAI Embeddings

## Prerequisite

1. [Log in](https://portal.azure.com/) or [sign up](https://azure.microsoft.com/en-us/free/) to Azure
2. [Create](https://portal.azure.com/#create/Microsoft.CognitiveServicesOpenAI) your Azure OpenAI and wait for approval approximately 10 business days
3. Your API key will be available at **Azure OpenAI** > click **name\_azure\_openai** > click **Click here to manage keys**

<figure><img src="../../../.gitbook/assets/azure/azure-general/1.png" alt=""><figcaption></figcaption></figure>

## Setup

### Azure OpenAI Embeddings

1. Click **Go to Azure OpenaAI Studio**

<figure><img src="../../../.gitbook/assets/azure/azure-general/2.png" alt=""><figcaption></figcaption></figure>

2. Click **Deployments**

<figure><img src="../../../.gitbook/assets/azure/azure-general/3.png" alt=""><figcaption></figcaption></figure>

3. Click **Create new deployment**

<figure><img src="../../../.gitbook/assets/azure/azure-general/4.png" alt=""><figcaption></figcaption></figure>

4. Select as shown below and click **Create**

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/1.png" alt="" width="559"><figcaption></figcaption></figure>

5. Successfully created **Azure OpenAI Embeddings**

* Deployment name: `text-embedding-ada-002`
* Instance name: `top right conner`

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/2.png" alt=""><figcaption></figcaption></figure>

### Flowise

1. **Embeddings** > drag **Azure OpenAI Embeddings** node

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/3.png" alt="" width="563"><figcaption></figcaption></figure>

2. **Connect Credential** > click **Create New**

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/4.png" alt="" width="386"><figcaption></figcaption></figure>

3. Copy & Paste each details (API Key, Instance & Deployment name, [API Version](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference#chat-completions)) into **Azure OpenAI Embeddings** credential

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/5.png" alt="" width="554"><figcaption></figcaption></figure>

4. Voila [🎉](https://emojipedia.org/party-popper/), you have created **Azure OpenAI Embeddings node** in Flowise

<figure><img src="../../../.gitbook/assets/azure/azure-general/5.png" alt=""><figcaption></figcaption></figure>

## Resources

* [LangChain JS Azure OpenAI Embeddings](https://js.langchain.com/docs/modules/data\_connection/text\_embedding/integrations/azure\_openai)
* [Azure OpenAI Service REST API reference](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference)
