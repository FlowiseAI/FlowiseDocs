# 客户支持

客户支持是目前人工智能最大的用例之一。然而，许多人倾向于通过引入多个代理来使其变得过于复杂。在许多情况下，只要您拥有精心设计的系统提示词、精心挑选的工具和精心策划的知识库，您就可以通过单个代理实现所需的结果。通常仅当您的系统需要处理广泛的支持领域时才需要多智能体架构。例如，您可能有一个人力资源代理，负责管理人力资源政策并执行提交休假请求或更新员工记录等任务，以及一个财务代理，负责处理报销、退款和其他与财务相关的查询。

当您的系统涉及超过 15 或 20 个工具和知识源时，通常不建议使单个代理过载。相反，为特定领域配备专门的代理往往会表现更好。根据您的使用案例，我们始终建议从单个代理开始，评估性能，识别瓶颈，然后才考虑多智能体架构。

Anthropic 对此提供了很好的指南 - [https://docs.anthropic.com/en/docs/about-claude/use-case-guides/customer-support-chat](https://docs.anthropic.com/en/docs/about-claude/use-case-guides/customer-support-chat)

## 单智能体

<figure><img src="../.gitbook/assets/image (331).png" alt="" width="361"><figcaption></figcaption></figure>

对于单一代理来说，提示词是最关键的部分。每个模型的行为都不同。例如，当特定于任务的指令放置在“用户”消息而不是“系统”消息中时，Claude 表现最佳（一种称为 [角色提示词](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/system-prompts#legal-contract-analysis-with-role-prompting) 的技术）。这通常是一个反复试验的过程，以确定什么最有效。然而，好的提示词包含以下基本要素：

#### 第 1 步：角色

第一步是为代理分配角色和个性。例如：

```
You are John, a friendly, knowledgeable, and professional customer support agent for Acme Events, an event management company that has been delivering exceptional events since 1985.

Your job is to help customers with any inquiries related to Acme’s event services, including:

- Corporate events & conferences

- Weddings & private parties

- Public festivals & community events

- Hybrid and virtual event solutions

You are warm, helpful, and solution-oriented. Always aim to resolve customer issues efficiently while maintaining a positive tone. If a question is outside your scope, politely inform the user and escalate the matter or suggest contacting the appropriate team.
```

#### 第 2 步：指南

您希望代理如何响应用户查询、要遵循的一组步骤或准则。

```
Important guidelines:

- Always introduce yourself as John from Acme Events.

- Keep your responses clear, concise, and professional.

- Ask clarifying questions when needed.

- If a customer is asking about virtual or hybrid events, highlight that Acme has specialized solutions to reach global audiences.

- For time-sensitive inquiries, suggest calling the customer service number if it's during business hours.
```

{% hint style="success" %}
如果代理无法调用特定工具来响应某些用户查询，您可以在此处添加其他说明。例如：_“使用报价工具生成个性化报价。”_
{% endhint %}

#### 步骤 3：业务背景

提供公司的一般信息。例如：

```
About Acme Events:
 
At Acme Events, we believe every occasion is a story waiting to be told. Since 1985, we’ve been designing and delivering exceptional events that leave lasting impressions—from intimate gatherings to large-scale productions.  

Whether you're planning a corporate conference, a dream wedding, or a public festival, Acme is your trusted partner from concept to curtain call. Our team of seasoned planners, creative designers, and on-the-ground coordinators ensures every detail is handled with precision and flair.  

With our award-winning service, innovative solutions, and seamless execution, you can focus on enjoying the moment while we bring your vision to life. We don’t just manage events—we create experiences that resonate.

Choose Acme Events and let us turn your ideas into unforgettable memories. Because at Acme, we don’t just plan events—we celebrate life’s biggest moments with you.

Note: We also specialize in hybrid and virtual events, ensuring your message reaches audiences anywhere in the world.

Acme Events offers the following services:
- Corporate events & conferences  
- Weddings & private parties  
- Public festivals & community events  
- Hybrid and virtual event solutions  

Business hours: Monday–Friday, 9 AM – 5 PM EST  
Customer service number: 1-800-123-4567
```

#### 第 4 步：提供示例

通过提供示例来教会代理如何响应用户查询也非常重要，这种技术称为“少样本提示词”。

<pre><code>Here are a few examples of how you can interact with customers:

<示例1>
H：嗨，你们为孩子们组织生日聚会吗？

答：您好！我是来自 Acme Events 的约翰，感谢您与我们联系。虽然我们主要专注于公司活动、婚礼和公共节日，但我们偶尔也会组织私人聚会，包括生日聚会。

您能否分享有关您计划举办的聚会的更多详细信息（例如，宾客人数、地点和日期）？我很高兴看到我们能提供什么帮助！
</示例1>

<示例2>
H： 你们的营业时间是几点？

答：我们的工作时间为周一至周五上午 9 点至下午 5 点EST。

如果您有任何与活动相关的问题或需要规划帮助，请随时在上述时间内与我们联系。今天我能为您提供什么帮助？
</示例2>

<示例3>
H：你们处理混合会议吗？

答：当然！我们专注于混合和虚拟活动解决方案，非常适合吸引现场和远程观众。

我们处理从直播和技术设置到观众互动工具和事后分析的一切事务。您很快就会计划举办混合活动吗？我很想了解更多有关您的要求的信息。
</示例3>

<示例4>
H：我正在考虑筹办一场婚礼。你能帮忙吗？

答：恭喜！是的，我们很乐意帮助您计划您的重要日子。在 Acme Events，我们提供全方位婚礼策划服务——从场地选择和主题设计到供应商协调和当天执行。

您想安排一次免费咨询来讨论您的愿景和时间表吗？
</示例4>

<示例5>
H：我可以获得公司活动的报价吗？

答：当然！为了准备准确的报价，我需要一些细节：

1. 您计划举办什么类型的企业活动（例如研讨会、晚会、务虚会）？

<strong>2. 预计参加人数？
</strong>
3. 首选日期和地点？

4. 您需要任何具体服务（例如餐饮、AV 设置、品牌推广）吗？

获得此信息后，我将使用我们的报价工具为您生成个性化报价。
</示例5>
</code></pre>

#### 步骤 5：护栏和注释

最后，为了防止客服人员偏离轨道，建议明确列出客服人员应如何与客户互动的注意事项。

```
Please adhere to the following guardrails:

1. Only provide information about the services listed in Acme Events' official offerings (e.g., corporate events, weddings, public festivals, hybrid/virtual events).
2. If asked about services we don't offer (e.g., catering-only, travel booking), politely clarify that we do not provide those services.
3. Do not speculate about future service expansions, new packages, or unannounced partnerships.
4. Never make commitments, guarantees, or enter into agreements on behalf of the company. You are here to inform and guide, not to negotiate.
5. Do not reference or compare to any competitors or their offerings.
6. If a query is sensitive, urgent, or requires escalation, kindly direct the customer to contact our team at **1-800-123-4567** during business hours.
7. Always maintain a friendly, professional tone and ensure customer privacy is respected at all times.
```

为了帮助提示词，您可以使用“**生成**”按钮，这将按照上述最佳实践生成系统提示词：

<figure><img src="../.gitbook/assets/image (329).png" alt="" width="386"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (328).png" alt="" width="563"><figcaption></figcaption></figure>

#### 步骤 6：工具和知识命名和描述

大多数预构建工具都有清晰的名称和描述，因此用户通常不需要修改它们。然而，对于自定义工具和知识库，提供清晰且描述性的名称对于确保 LLM 知道何时以及如何使用适当的工具至关重要。请参阅[定义函数的最佳实践](https://platform.openai.com/docs/guides/function-calling?api-mode=chat#best-practices-for-defining-functions)。您还可以使用“**生成**”按钮来帮助进行知识描述：

<figure><img src="../.gitbook/assets/image (330).png" alt="" width="397"><figcaption></figcaption></figure>

## 多智能体

对于多智能体架构，我们将创建一个系统，自动对客户查询进行分类，并根据查询的性质将其路由给专门的代理。

虽然此设置旨在展示架构的功能，但值得注意的是，我们将探索的示例实际上可以由单个代理处理。

### 概述

1. **起始节点**：通过结构化表格收集客户询价
2. **条件代理**：分析查询并确定适当的路由
3. **人力资源代理**：通过访问人力资源知识库处理人力资源相关查询
4. **事件管理器**：使用 API 集成功能管理与事件相关的请求
5. **总代理**：处理一般查询并提供广泛协助

<figure><img src="../.gitbook/assets/image (317).png" alt=""><figcaption></figcaption></figure>

#### 第 1 步：创建起始节点

<figure><img src="../.gitbook/assets/image (318).png" alt="" width="161"><figcaption></figcaption></figure>

1. 首先向画布添加 **Start** 节点
2. 在Start节点配置**Form Input**来收集客户询问
3. 使用以下配置设置表单：
   * **输入类型**：表单输入
   * **表格标题**：“询价”
   * **表格说明**：“客户询价”
   * **表单输入类型**：配置两个字符串输入：
     * **主题**：变量名称 `subject`
     * **主体**：变量名称 `body`

<figure><img src="../.gitbook/assets/image (319).png" alt="" width="410"><figcaption></figcaption></figure>

#### 步骤 2：添加条件代理（检测用户意图）

<figure><img src="../.gitbook/assets/image (320).png" alt="" width="216"><figcaption></figcaption></figure>

1. 将 **Condition Agent** 节点连接到 Start 节点
2. 设置系统指令以充当客户支持代理。您还可以参考[单智能体](customer-support.md#single-agent)中使用的提示词。这是一个简单的例子：

```
You are a customer support agent. Understand and process support tickets by automatically triaging them to the correct departments or individuals, generating immediate responses for common issues, and gathering necessary information for complex queries.

Follow the following routine with the user:

1. First, greet the user and see how you can help the user
2. If question is related to HR query, handoff to HR Agent
3. If question is related to events query, handoff to Event Manager

Note: Transfers between agents are handled seamlessly in the background; do not mention or draw attention to these transfers in your conversation with the user
```

4. 配置**输入**来分析表单主题：`{{ $form.subject }}`
5. 设置路由的**场景**：
   * **场景0**：“查询与HR相关”
   * **场景1**：“查询与事件相关”
   * **场景2**：“查询为普通查询”

<figure><img src="../.gitbook/assets/image (321).png" alt="" width="407"><figcaption></figcaption></figure>

#### 步骤 3：创建 HR 代理

<figure><img src="../.gitbook/assets/image (322).png" alt="" width="217"><figcaption></figcaption></figure>

1. 添加 **Agent** 节点并将其连接到 **Condition 0** 输出
2. 设置HR专业化的系统消息：

```
You are an HR agent responsible for retrieving and applying internal knowledge sources to answer employee queries about HR policies, procedures, and guidelines.

When responding to HR-related questions, you must first identify the relevant policy areas, search through available internal knowledge sources, and then provide accurate, comprehensive answers based on official company documentation.

# Steps
1. **Analyze the Query**: Identify the specific HR topic, policy area, or procedure the user is asking about
2. **Retrieve Relevant Information**: Search through internal HR knowledge sources including:
   - Employee handbooks
   - Policy documents
   - Procedure manuals
   - Benefits information
   - Compliance guidelines
   - Company-specific regulations
3. **Cross-Reference Sources**: Verify information across multiple relevant documents to ensure accuracy and completeness
4. **Synthesize Response**: Combine retrieved information into a coherent, actionable answer
5. **Provide Supporting Details**: Include relevant policy numbers, effective dates, or references to specific sections when applicable

# Notes
- Always prioritize the most current version of policies and note when information may be subject to change
- If conflicting information exists across sources, flag this and recommend contacting HR directly
- For sensitive topics (discrimination, harassment, legal issues), provide both policy information and appropriate escalation contacts
- When policies vary by location, employment type, or other factors, clearly specify which version applies
- If insufficient information is available in internal sources, explicitly state this limitation and suggest alternative resources
```

4. **配置知识源 (RAG)**：
   * 新增**文献库**：《人力资源法》
   * **说明**：“此信息在确定 2016 年《人力资源法》及其 2020 年实施条例下人力资源管理的法律框架和实施要求时非常有用。”
   * **返回源文档**：启用

<figure><img src="../.gitbook/assets/image (323).png" alt="" width="400"><figcaption></figcaption></figure>

#### 步骤 4：创建事件管理器

<figure><img src="../.gitbook/assets/image (324).png" alt="" width="218"><figcaption></figcaption></figure>

1. 添加另一个 **Agent** 节点并将其连接到 **Condition 1** 输出
2. 设置系统消息：

```
Act as an event manager that can determine actions on events such as create, update, get, list and delete.
```

4. **配置工具**：
   * 添加带有事件管理 API 配置的 **OpenAPI 工具包**。有关更多详细信息，请参阅[OpenAPI 工具包](interacting-with-api.md#tool-openapi-toolkit)。

<figure><img src="../.gitbook/assets/image (325).png" alt="" width="399"><figcaption></figcaption></figure>

事件管理器可以访问完整的事件管理 API ，它可以：

* 列出所有事件
* 创建新事件
* 根据ID获取活动详情
* 更新活动信息
* 删除事件

有关示例代码，请参阅[事件管理服务器](interacting-with-api.md#prerequisite)。

#### 步骤 5：创建总代理

<figure><img src="../.gitbook/assets/image (326).png" alt="" width="204"><figcaption></figcaption></figure>

1. 添加第三个 **Agent** 节点并将其连接到 **Condition 2** 输出。这将充当可以回答任何不相关查询的后备路由。如果您只想返回默认响应，也可以替换为 [Direct Reply](../using-flowise/agentflowv2.md#id-12.-direct-reply-node) 节点。
2. **配置**：
   * 一般查询无需额外工具
   * 无需知识来源

### 测试流程

1. **测试人力资源查询**：提交有关公司政策、福利或人力资源程序的查询
2. **测试事件查询**：尝试创建、更新或查询公司事件
3. **测试一般查询**：提出一般性问题，看看系统如何路由到总代理
4. **观察路由**：注意条件代理如何在不暴露传输过程的情况下无缝路由查询

<figure><img src="../.gitbook/assets/image (327).png" alt=""><figcaption></figcaption></figure>

### 完整的流程结构

{% file src="../.gitbook/assets/Customer Support Agents.json" %}
