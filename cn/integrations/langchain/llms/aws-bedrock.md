---
description: 围绕 AWS 基岩大型语言模型的包装。
---

# AWS 基岩


**AWS Bedrock LLM** 节点与 Amazon Bedrock 集成，这是一项托管服务，提供对用于构建生成 AI 应用程序的基础模型的访问。 

要在 Flowise 中使用 AWS Bedrock，请将 **AWS Bedrock LLM** 节点添加到您的 聊天流。 

# 设置

1. 为 Flowise 配置 AWS 凭据。

在 Amazon Bedrock 中，确保您的账户有权访问您想要与 Flowise 一起使用的模型。 

2. 将 AWS Bedrock 节点添加到您的 聊天流。

在 Flowise 画布中，将 AWS Bedrock LLM 节点拖放到您的 聊天流 中。 

3. 配置 AWS Bedrock 输入：
<figure><img src="../../../.gitbook/assets/image (2) (5).png" alt="" width="275"><figcaption><p>AWS 基岩节点</p></figcaption></figure>

* AWS 凭据：选择现有 AWS 凭据或创建一个新凭据。关联的 IAM 用户或角色必须拥有 `bedrock:InvokeModel` 以及 聊天流 中任何其他所需 AWS 服务的权限。
* 区域：您的基岩模型可用的 AWS 区域。
* 模型名称：对话式 AI 的 AWS 基岩基础模型。

4. 连接 聊天流 中的 AWS Bedrock 节点。

添加其他 聊天流 组件（例如输入节点、输出节点、内存节点）后，将 AWS Bedrock LLM 节点连接到相应的组件以创建 聊天流。

有关在 AWS 上部署 Flowise 的信息，请参阅 [AWS](../../../configuration/deployment/aws.md)。


