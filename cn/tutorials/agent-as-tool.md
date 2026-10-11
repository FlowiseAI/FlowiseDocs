# 代理作为工具

在本教程中，我们将了解如何利用其他流作为父代理的工具。这种方法允许您创建一个父代理，该父代理可以将特定任务委托给专门的子代理

## 概述

1.通过父代理接收用户输入
2. Agent 决定从文档存储中检索数据，或调用 智能体流程 工具。

<figure><img src="../.gitbook/assets/image (295).png" alt="" width="375"><figcaption></figcaption></figure>

### 第 1 步：设置起始节点

首先向画布添加 **Start** 节点。这是您的代理系统的入口点。

### 步骤 2：创建父代理

添加 **Agent** 节点并将其连接到 Start 节点。

### 步骤 3：配置代理工具

此流程的关键功能是将另一个代理配置为工具。在父代理的 **工具** 部分中：

<figure><img src="../.gitbook/assets/image (296).png" alt="" width="354"><figcaption></figcaption></figure>

#### 工具配置：

* **工具**：选择“**代理作为工具**”

#### 代理工具设置：

* **选定的智能体流程**：选择您的子智能体流程
* **名称**：智能体流程的名称
* **描述**：描述此智能体流程何时有用。示例：

```
Useful for searching user availability, scheduling meetings and email related query
```

{% hint style="warning" %}
该工具的名称和描述非常重要！它们必须清晰并正确地描述该工具的用途。请参阅[最佳实践](https://platform.openai.com/docs/guides/function-calling?api-mode=chat#best-practices-for-defining-functions)指南。
{% endhint %}

### 步骤4：添加知识源

配置 **知识（文档存储）** 部分以使您的父代理能够访问相关信息。这与 [RAG](rag.md) 教程相同。

<figure><img src="../.gitbook/assets/image (297).png" alt="" width="518"><figcaption></figcaption></figure>

#### 文档存储配置：

* **文档存储**：选择您预先配置的文档存储（例如“AI-Paper”）
* **描述知识**：描述知识的内容

***

## 交互示例

#### 示例查询和预期行为：

**调度查询：**

* 用户：“你能查一下我下周二有空吗？”
* 流程：家长代理→个人\_助理工具→专业调度响应

<figure><img src="../.gitbook/assets/image (301).png" alt="" width="563"><figcaption></figcaption></figure>

**技术查询：**

* 用户：“什么是 AIGC 以及它是如何工作的？”
* 流程：父代理 → AI-Paper 知识库 → 技术解释及来源

<figure><img src="../.gitbook/assets/image (300).png" alt="" width="563"><figcaption></figcaption></figure>

**一般查询：**

* 用户：“你好，你好吗？”
* 流程：父代理 → 直接响应（无需工具）

**复杂查询：**

* 用户：“下周二安排一次关于 AIGC 实施的会议，提取关键见解和谈话要点”
* Flow：家长代理→既个人\_辅助工具ANDAI-Paper知识→协调响应

<figure><img src="../.gitbook/assets/image (302).png" alt="" width="563"><figcaption></figcaption></figure>

***

## 最佳实践

#### 设计指南：

1. **清晰的工具描述**：使工具名称和描述具体且可操作
2. **适当的委派**：更好的系统提示词父代理有效委派

#### 常见用例：

* **客户服务**：家长代理拥有用于计费、技术支持和一般查询的专用工具
* **研究助理**：家长拥有不同研究领域（法律、技术、市场研究）的工具
* **项目管理**：父级提供日程安排、资源分配和进度跟踪工具
* **内容创建**：家长拥有用于写作、编辑、研究和格式化的工具

