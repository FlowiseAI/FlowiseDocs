---
description: Autonomous agent with chain of thoughts for self-guided task completion.
---

# AutoGPT

<figure><img src="../../../.gitbook/assets/image (12) (2).png" alt="" width="277"><figcaption><p>AutoGPT Node</p></figcaption></figure>
# AutoGPT Agent Functionality

The AutoGPT Agent in FlowWise AI is designed to automate the use of multiple tools to perform complex tasks autonomously. By utilizing this agent, users can define high-level objectives, and the AutoGPT Agent will orchestrate various sub-tasks using available tools, without requiring user intervention at every step. This makes it ideal for workflows that require decision-making, iteration, and integration with different tools or data sources.

For example, the AutoGPT Agent can be used for:

- Researching a topic across multiple sources and summarizing findings.
- Planning a project by breaking down goals into actionable steps and gathering relevant information.
- Automating marketing tasks, such as drafting emails, social media posts, and content creation, based on a given strategy.

This functionality helps users accomplish intricate tasks that typically require multiple steps and dynamic adjustments, allowing for a highly efficient, hands-off experience.

**Inputs**

The inputs to the Auto-GPT agent in FlowWise depend on the specific goals and tasks you want the agent to autonomously perform. Typically, Auto-GPT requires several key inputs to operate effectively, which guide its reasoning and task completion. Here are the common inputs that an Auto-GPT agent can take:

### Common Inputs for Auto-GPT Agent:

1. **Initial Goal or Task**:
   - The Auto-GPT agent requires a clear **goal** or **task description** that defines what it should achieve. This input is the starting point for the agent’s autonomous reasoning process. The task could be anything from "Research the best marketing strategies for 2024" to "Generate a business plan for an e-commerce startup."

2. **Sub-Tasks or Objectives** (Optional):
   - You can input **sub-tasks** or **milestones** that help guide the Auto-GPT agent toward achieving the main goal. These can help break down complex tasks into manageable steps. For example, if the main goal is "Create a content strategy," sub-tasks might be "Research audience demographics," "Analyze competitor content," and "Draft a content calendar."

3. **Context or Constraints**:
   - **Contextual information** or **constraints** can be provided as input to shape how the Auto-GPT agent approaches the task. This could include things like budget limits, time constraints, company-specific guidelines, or a particular focus area. For example, “Create a marketing plan with a budget of $10,000” or “Generate content ideas for a tech audience.”

4. **Prompt or Instruction Input**:
   - Similar to other AI models, the Auto-GPT agent can be provided with **instructions or prompts** that guide its immediate behavior. This could be a question, a problem to solve, or a scenario. For example, "Analyze the market trends for renewable energy in 2024" or "Develop a three-step plan to improve customer retention."

5. **Memory or Previous Outputs** (Optional):
   - **Previous data** or **memory** from earlier conversations or tasks can be inputted into the Auto-GPT agent to build on prior work. If the agent has been working on a task over multiple sessions, inputting previous results helps maintain continuity.

6. **Tool Access Information** (Optional):
   - The Auto-GPT agent can take **tool access details** as inputs if it needs to interact with external tools, databases, or APIs to complete the task. For example, if the agent is automating research, it might need access to APIs for search engines or databases where it retrieves information.

7. **Custom Parameters**:
   - You can define **custom parameters** that the agent uses for the task at hand. These could be specific values like datasets, links to files, or customer-specific details. For example, "Fetch sales data for Q1 2024 from this spreadsheet" or "Analyze the user reviews from the following source."

8. **Data for Processing** (Optional):
   - If you want the agent to perform tasks such as data analysis, you can input **structured or unstructured data** for the Auto-GPT agent to process. This could be CSV files, text inputs, or API data that the agent analyzes to provide insights.

### Example Use Case Inputs:
- **Task**: “Research 2024 marketing trends and create a report”
- **Sub-Tasks**:
   1. Identify top marketing trends.
   2. Provide a list of key statistics and figures.
   3. Create a 5-step actionable plan based on the findings.
- **Context**: “Focus on social media marketing trends under a $10,000 budget.”
- **Tools**: Provide access to market research databases or APIs.

### Flowise Nodes to Input Data:
You can pass these inputs into the Auto-GPT agent via different nodes in Flowise, such as:
- **Prompt Templates**: To define goals or tasks.
- **Input Nodes**: For user-provided data or interactive sessions.
- **APIs/Tools**: To provide the agent with additional resources (e.g., data sources or external APIs for research).

In summary, the inputs to the Auto-GPT agent include a goal or task description, optional sub-tasks, contextual constraints, specific prompts or instructions, memory from previous tasks, access to external tools or APIs, and custom parameters or data for processing. These inputs help the Auto-GPT agent function autonomously to achieve complex tasks in Flowise.

**Output**

The output from the AutoGPT Agent is provided through the **AutoGPT**, which compiles the results from each tool or task involved in the process. The output includes a structured plan, intermediate results from each step, and the final outcome. This allows for easy review of the agent's actions and the decisions made during the process, making it a powerful tool for complex, multi-stage tasks.

### Possible Output Connections for Auto-GPT Agent
The output of the Auto-GPT agent can be connected to several other nodes and tools, allowing for complex workflows and automations. Here are some common ways to use the output of the Auto-GPT agent:

1. **Display/UI Nodes**:
   - You can connect the output of the Auto-GPT agent to display nodes to show the results of its reasoning, suggestions, or task completion in a chatbot, website, or app interface. For example, if Auto-GPT completed a research task or generated a plan, the result can be presented to the user.

2. **Custom Tools**:
   - The Auto-GPT output can be directed to custom tools for further processing. For example, if Auto-GPT generates a list of actions, these actions can be fed into a custom tool to trigger specific workflows (like adding tasks to a task management system or sending data to another API).

3. **API or Webhook Nodes**:
   - You can connect the Auto-GPT agent to API nodes or webhooks to send data or trigger actions in external systems. For example, if Auto-GPT comes up with a marketing plan, you could automatically push that plan to a system like Google Docs, a project management tool, or a CRM via an API.

4. **Logic or Conditional Nodes**:
   - The output from Auto-GPT can be processed through logic or conditional nodes to make decisions based on its output. If Auto-GPT suggests multiple steps or options, these conditions can be evaluated to choose the next best course of action in the flow.

5. **Database or Storage Nodes**:
   - The results from Auto-GPT, such as structured plans or actions, can be stored in a database (e.g., Airtable, MongoDB, or a local database) for further use or future reference. This is useful if Auto-GPT generates ongoing reports or logs decisions that need to be tracked.

6. **Email or Messaging Tools**:
   - Auto-GPT’s output can trigger email notifications (e.g., using a tool like SendGrid) or messages to platforms like Slack or Discord. For example, after Auto-GPT completes a business analysis or a task like summarizing research, the results can be emailed to stakeholders or posted in a team channel.

7. **CRM Systems or Task Management**:
   - The output of Auto-GPT can feed into CRM systems (such as Salesforce, HubSpot, or Zoho) to create leads, tasks, or records based on its autonomous reasoning. For example, Auto-GPT could analyze incoming data and generate actions or follow-ups in your CRM.

8. **File Handling Nodes**:
   - If Auto-GPT produces large outputs such as reports, documents, or datasets, it can be connected to file handling nodes (e.g., exporting to Google Sheets, CSVs, or storing in cloud storage systems like AWS or Google Cloud).

9. **External AI Models or Agents**:
   - The output from the Auto-GPT agent can be routed to other AI agents or tools for further refinement. For example, if Auto-GPT generates a summary or code, it can be passed to another model for deeper analysis or debugging.

10. **Task or Process Automation Systems**:
   - Auto-GPT can integrate with automation platforms like Zapier, Integromat, or Make.com, triggering multi-step automations based on its output. For example, after completing a business task, Auto-GPT can automatically set up meetings, send emails, and update documents.

### Example Use Cases
- **Autonomous Research**: After Auto-GPT completes a research task, the results can be emailed to a team, displayed in a chatbot, or stored in a knowledge base for future use.
- **Project Automation**: Auto-GPT generates a project plan, which can be sent to a task management system like Trello or Asana to create tasks.
- **Code Generation**: If Auto-GPT writes code, that output can be connected to a custom tool or an API that deploys the code or sends it for review.

In summary, the Auto-GPT agent’s output can connect to a wide variety of nodes, tools, and systems depending on your goals, including display nodes, custom tools, APIs, conditional logic, CRMs, databases, task managers, and automation systems. This flexibility enables powerful autonomous workflows.

{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](../../../contributing/) to get started.
{% endhint %}
