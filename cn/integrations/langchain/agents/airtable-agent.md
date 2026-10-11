---
description: 代理用于回答 Airtable 表上的查询。
---

# 空中桌代理

<figure><img src="../../../.gitbook/assets/image_airtable.png" alt="" width="271"><figcaption><p>Airtable代理节点</p></figcaption></figure>

## Airtable 代理功能

Airtable Agent 旨在促进 Flowise AI 和 Airtable 表之间的交互，使用户能够以对话方式查询 Airtable 数据。通过使用该代理，用户可以询问有关 Airtable 库内容的问题，并根据存储的数据接收相关响应。这对于快速提取特定信息、自动化工作流程或从 Airtable 中存储的数据生成摘要特别有用。

例如，Airtable Agent 可用于回答以下问题：

*“我的项目跟踪表中还有多少任务未完成？”
*“CRM 中列出的客户的联系方式是什么？”
*“给我一份过去一周添加的所有记录的摘要。”

此功能可帮助用户从 Airtable 库中获取见解，而无需浏览 Airtable 界面，从而更轻松地以无缝、交互式的方式管理和分析数据。

## 输入

Airtable Agent 需要以下输入才能有效运行：

* **语言模型**：用于处理查询的语言模型。此输入是必需的，有助于确定代理提供的响应的质量和准确性。
* **输入审核**：启用内容审核的可选输入。这有助于确保查询适当且不包含攻击性或有害内容。
* **连接凭据**：连接到 Airtable 所需的输入。用户必须选择有权访问其 Airtable 数据的适当凭据。
* **底座 ID**：要连接的 Airtable 底座的 ID。这是必填字段，可以在 Airtable API 文档或基本设置中找到。如果您的表 URL 类似于 `https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYlCO/viw9UrP77idOCE4ee`，则 `app11RobdGoX0YNsC` 是基本 ID。用于指定哪个Airtable库包含要查询的数据。
* **桌子 ID**：Airtable 底座内特定桌子的 ID。这也是必填字段，可帮助代理针对正确的表进行数据检索。在示例 URL `https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYlCO/viw9UrP77idOCE4ee` 中，`tblJdmvbrgizbYlCO` 是表 ID。
* **附加参数**：可用于自定义代理行为的可选参数。可以根据具体用例配置这些参数。
  * **返回全部**：此选项允许用户返回指定表中的所有记录。如果启用，将检索所有记录，否则，仅返回有限数量的记录。
  * **限制**：指定在未启用**全部返回**时要返回的最大记录数。默认值为 `100`。

**注意**：本节是一项正在进行的工作。我们感谢您在完成本节时提供的任何帮助。请查看我们的[贡献指南](/broken/pages/G48tdmpQ3z4CTWEspqkA)以开始使用。
