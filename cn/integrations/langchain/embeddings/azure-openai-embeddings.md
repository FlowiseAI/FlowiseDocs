# Azure OpenAI 嵌入

## 先决条件

1. [登录](https://portal.azure.com/)或[注册](https://azure.microsoft.com/en-us/free/)到 Azure
2. [创建](https://portal.azure.com/#create/Microsoft.CognitiveServicesOpenAI)您的 Azure OpenAI 并等待大约 10 个工作日的批准
3. 您的 API 密钥将在 **Azure OpenAI** 中提供 > 单击 **名称\_azure\_openai** > 单击 **单击此处管理密钥**

<figure><img src="../../../.gitbook/assets/azure/azure-general/1.png" alt=""><figcaption></figcaption></figure>

## 设置

### Azure OpenAI 嵌入

1. 单击“**转到 Azure OpenaAI Studio**”

<figure><img src="../../../.gitbook/assets/azure/azure-general/2.png" alt=""><figcaption></figcaption></figure>

2. 单击**部署**

<figure><img src="../../../.gitbook/assets/azure/azure-general/3.png" alt=""><figcaption></figcaption></figure>

3. 单击“**创建新部署**”

<figure><img src="../../../.gitbook/assets/azure/azure-general/4.png" alt=""><figcaption></figcaption></figure>

4.如下图选择，点击**创建**

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/1.png" alt="" width="559"><figcaption></figcaption></figure>

5. 成功创建**Azure OpenAI Embeddings**

* 部署名称：`text-embedding-ada-002`
* 实例名称：`top right conner`

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/2.png" alt=""><figcaption></figcaption></figure>

### 流动

1. **嵌入** > 拖动 **Azure OpenAI Embeddings** 节点

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/3.png" alt="" width="563"><figcaption></figcaption></figure>

2. **连接凭据** > 单击 **新建**

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/4.png" alt="" width="386"><figcaption></figcaption></figure>

3. 将每个详细信息（API 密钥、实例和部署名称、[API 版本](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference#chat-completions)）复制并粘贴到 **Azure OpenAI Embeddings** 凭据中

<figure><img src="../../../.gitbook/assets/azure/azure-openai-embeddings/5.png" alt="" width="554"><figcaption></figcaption></figure>

4.瞧[🎉](https://emojipedia.org/party-popper/)，您已在 Flowise 中创建了 **Azure OpenAI Embeddings 节点**

<figure><img src="../../../.gitbook/assets/azure/azure-general/5.png" alt=""><figcaption></figcaption></figure>

## 资源

* [LangChain JS Azure OpenAI 嵌入](https://js.langchain.com/docs/modules/data\_connection/text\_embedding/integrations/azure\_openai)
* [Azure OpenAI 服务 REST API 参考](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference)
