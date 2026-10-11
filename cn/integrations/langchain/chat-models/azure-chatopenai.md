# 聊天OpenAI

## 先决条件

1. [OpenAI](https://openai.com/) 帐户
2. 创建 [API 密钥](https://platform.openai.com/api-keys)

## 设置

1. **聊天模型** > 拖动 **ChatOpenAI** 节点

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. **连接凭据** > 单击 **新建**

<figure><img src="../../../.gitbook/assets/image_openAI (1).png" alt="" width="278"><figcaption></figcaption></figure>

2.填写**ChatOpenAI**凭据

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

4.瞧[🎉](https://emojipedia.org/party-popper/)，您现在可以在 Flowise 中使用 **ChatOpenAI 节点**

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 自定义基础 URL 和标头

Flowise 支持使用自定义基础 URL 和 Chat OpenAI 标头。用户可以轻松使用 OpenRouter、TogetherAI 等集成，以及其他支持 OpenAI API 兼容性的集成。

### 一起AI

1. 参考TogetherAI官方[docs](https://docs.together.ai/docs/openai-api-compatibility#nodejs)
2.使用TogetherAI API密钥创建新凭据
3. 单击 ChatOpenAI 节点上的“**其他参数**”。
4. 更改基本路径：

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

### 打开路由器

1. 参考OpenRouter官方[docs](https://openrouter.ai/docs#quick-start)
2. 使用 OpenRouter API key 创建新凭据
3. 点击ChatOpenAI节点上的附加参数
4. 更改基本路径和基本选项：

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## 定制模型

对于 ChatOpenAI 节点不支持的模型，您可以使用 ChatOpenAI Custom。这允许用户填写模型名称，例如 `mistralai/Mixtral-8x7B-Instruct-v0.1`

<figure><img src="../../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

## 图片上传

您还可以允许 LLM 上传和分析图像。在底层，Flowise 将使用 [OpenAI Vison](https://platform.openai.com/docs/guides/vision) 模型来处理图像。仅适用于 LLMChain、Conversation Chain、ReAct Agent 和 Conversational Agent。

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="332"><figcaption></figcaption></figure>

在聊天界面中，您现在将看到一个新的图像上传按钮：

<figure><img src="../../../.gitbook/assets/Untitled (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>
