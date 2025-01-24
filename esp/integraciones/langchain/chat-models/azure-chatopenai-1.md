# Azure ChatOpenAI

## Prerequisitos

1. [Inicia sesión](https://portal.azure.com/) o [regístrate](https://azure.microsoft.com/en-us/free/) en Azure
2. [Crea](https://portal.azure.com/#create/Microsoft.CognitiveServicesOpenAI) tu Azure OpenAI y espera la aprobación aproximadamente 10 días hábiles
3. Tu clave API estará disponible en **Azure OpenAI** > haz clic en **name_azure_openai** > haz clic en **Click here to manage keys**

<figure><img src="../../../.gitbook/assets/azure/azure-general/1.png" alt=""><figcaption></figcaption></figure>

## Configuración

### Azure ChatOpenAI

1. Haz clic en **Go to Azure OpenaAI Studio**

<figure><img src="../../../.gitbook/assets/azure/azure-general/2.png" alt=""><figcaption></figcaption></figure>

2. Haz clic en **Deployments**

<figure><img src="../../../.gitbook/assets/azure/azure-general/3.png" alt=""><figcaption></figcaption></figure>

3. Haz clic en **Create new deployment**

<figure><img src="../../../.gitbook/assets/azure/azure-general/4.png" alt=""><figcaption></figcaption></figure>

4. Selecciona como se muestra a continuación y haz clic en **Create**

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/1.png" alt="" width="558"><figcaption></figcaption></figure>

5. Se ha creado exitosamente **Azure ChatOpenAI**

* Nombre del deployment: `gpt-35-turbo`
* Nombre de la instancia: `esquina superior derecha`

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/2.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/azure/azure-general/2.png" alt=""><figcaption></figcaption></figure>

### Flowise

1. **Chat Models** > arrastra el nodo **Azure ChatOpenAI**

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/3.png" alt="" width="563"><figcaption></figcaption></figure>

2. **Connect Credential** > haz clic en **Create New**

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/4.png" alt="" width="421"><figcaption></figcaption></figure>

3. Copia y pega cada detalle (API Key, Instance & Deployment name, [API Version](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference#chat-completions)) en la credencial de **Azure ChatOpenAI**

<figure><img src="../../../.gitbook/assets/azure/azure-chatopenai/5.png" alt="" width="563"><figcaption></figcaption></figure>

4. ¡Voilà [🎉](https://emojipedia.org/party-popper/), has creado el **nodo Azure ChatOpenAI** en Flowise

<figure><img src="../../../.gitbook/assets/azure/azure-general/5.png" alt=""><figcaption></figcaption></figure>

## Recursos

* [LangChain JS Azure ChatOpenAI](https://js.langchain.com/docs/modules/model_io/models/chat/integrations/azure)
* [Referencia de la API REST de Azure OpenAI Service](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference)
