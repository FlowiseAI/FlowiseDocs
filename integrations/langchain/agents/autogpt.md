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

## Inputs

The AutoGPT Agent requires the following inputs to function effectively:

- **Allowed Tools**: A list of tools that the agent is permitted to use to achieve its goal. This input is required and defines the capabilities available to the agent, such as web scraping, data analysis, or email sending. Examples of allowed tools include:
  - **Web Scraper**: To gather information from websites.
  - **Email Sender**: To send automated emails as part of a workflow.
  - **Data Loader**: To extract and load data from databases.
  - **API Integrator**: To connect with external APIs for retrieving or sending data.
  - **File Handler**: To read from or write to files.
- **Chat Model**: The language model to be used for processing queries and generating responses. This input is required to ensure the quality of generated content and decision-making.
- **Vector Store Retriever**: A retriever that allows the agent to pull information from a vector database. This input is required to provide the agent with access to relevant data and context during the task.
- **Input Moderation**: Optional input that enables content moderation. This helps ensure that queries are appropriate and do not contain offensive or harmful content.
- **AutoGPT Name**: An optional name for the AutoGPT Agent, which can be used for personalization or identification purposes during interactions.
- **AutoGPT Role**: Optional input that defines the role or persona of the agent (e.g., Assistant, Researcher). This helps shape the type of responses and approach the agent takes in fulfilling the goal.
- **Maximum Loop**: Specifies the maximum number of iterations or loops that the agent can execute. This optional input helps control how exhaustive the agent's process should be, with a default value of `5`.

## Output

The output from the AutoGPT Agent is provided through the **AgentExecutor**, which compiles the results from each tool or task involved in the process. The output includes a structured plan, intermediate results from each step, and the final outcome. This allows for easy review of the agent's actions and the decisions made during the process, making it a powerful tool for complex, multi-stage tasks.

The output from the AutoGPT Agent can also be connected to other modules within FlowWise to enhance the overall workflow. Examples of possible connections include:

- **Conversational Agent**: The output can be fed into a Conversational Agent to provide users with a summary or explanation of the tasks performed and the results obtained.
- **CSV Agent**: If the output includes data that needs further processing or storage, it can be sent to the CSV Agent to export the data into a CSV file for easier handling and sharing.
- **Airtable Agent**: The final results can be used to update Airtable records or add new entries using the Airtable Agent, making it possible to maintain up-to-date records based on the AutoGPT Agent's findings.
- **MistralAI Tool Agent**: If further analysis or advanced modeling is required, the output can be passed to the MistralAI Tool Agent for additional processing.
- **Notification Agent**: The final outcomes or any important intermediate results can be connected to a Notification Agent to alert relevant stakeholders via email or other communication channels.

These integrations help create seamless, end-to-end workflows that leverage the strengths of multiple agents within FlowWise, providing a highly efficient and automated solution for complex tasks.


{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](../../../contributing/) to get started.
{% endhint %}
