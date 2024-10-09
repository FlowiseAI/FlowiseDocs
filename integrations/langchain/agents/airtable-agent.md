---
description: Agent used to to answer queries on Airtable table.
---

# Airtable Agent

<figure><img src="../../../.gitbook/assets/image_airtable.png" alt="" width="271"><figcaption><p>Airtable Agent Node</p></figcaption></figure>
## Airtable Agent Functionality

The Airtable Agent is designed to facilitate interactions between FlowWise AI and Airtable tables, enabling users to query Airtable data in a conversational manner. By using this agent, users can ask questions about the contents of their Airtable base and receive relevant responses based on the stored data. This can be particularly useful for quickly extracting specific pieces of information, automating workflows, or generating summaries from the data stored in Airtable.

For example, the Airtable Agent can be used to answer questions like:

- "How many tasks are still incomplete in my project tracker table?"
- "What are the contact details of the clients listed in the CRM?"
- "Give me a summary of all records added in the past week."

This functionality helps users get insights from their Airtable bases without needing to navigate through the Airtable interface, making it easier to manage and analyze their data in a seamless, interactive way.

## Inputs

The Airtable Agent requires the following inputs to function effectively:

- **Language Model**: The language model to be used for processing queries. This input is required and helps determine the quality and accuracy of responses provided by the agent.
- **Input Moderation**: Optional input that enables content moderation. This helps ensure that queries are appropriate and do not contain offensive or harmful content.
- **Connect Credential**: Required input to connect to Airtable. Users must select the appropriate credential that has permissions to access their Airtable data.
- **Base ID**: The ID of the Airtable base to connect to. This is a required field and can be found in the Airtable API documentation or the base settings. If your table URL looks like `https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYlCO/viw9UrP77idOCE4ee`, `app11RobdGoX0YNsC` is the Base ID. It is used to specify which Airtable base contains the data to be queried.
- **Table ID**: The ID of the specific table within the Airtable base. This is also a required field and helps the agent target the correct table for data retrieval. In the example URL `https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYlCO/viw9UrP77idOCE4ee`, `tblJdmvbrgizbYlCO` is the Table ID.
- **Additional Parameters**: Optional parameters that can be used to customize the behavior of the agent. These parameters can be configured based on specific use cases.
  - **Return All**: This option allows users to return all records from the specified table. If enabled, all records will be retrieved, otherwise, only a limited number will be returned.
  - **Limit**: Specifies the maximum number of records to be returned if **Return All** is not enabled. The default value is `100`.

## Output

The output from the Airtable Agent is provided through the **AgentExecutor**, which handles the execution of the query and returns the relevant information. The output includes a **BaseChain** that manages the flow of data between Airtable and the agent, making it a runnable component that can be integrated into workflows to facilitate seamless data retrieval and response generation.

### Possible Outputs for Airtable Agent
The output of the Airtable Agent in FlowWise can connect to various nodes and other tools, depending on the type of interaction or integration you're building. The Airtable Agent is primarily used to manage data in Airtable, which can involve creating, reading, updating, or deleting records in an Airtable base. The outputs from these operations can be directed to several types of actions and nodes in FlowWise:

1. **Display Nodes**:
   - You can connect the output to a display or UI node to show the results of a query (e.g., retrieving data from an Airtable base and displaying it to the user in a chatbot or web interface).

2. **Custom Tools or Functions**:
   - The output can be sent to custom tools where further processing happens (e.g., sending retrieved data from Airtable to another API, integrating with other services like Google Sheets, or performing logic-based actions).

3. **Logic or Condition Nodes**:
   - Use logic nodes to branch flows based on the results from Airtable. For example, depending on whether a certain record exists or meets specific conditions, you can trigger different actions or paths in the workflow.

4. **Storage/Database Nodes**:
   - After retrieving or processing data from Airtable, the output can be saved or passed along to other databases, systems, or external services for further use.

5. **Webhook or API Nodes**:
   - You can connect the Airtable Agent's output to API calls or webhooks. For example, you could use the retrieved data to send an API request to another service or notify another system based on what was found or updated in Airtable.

6. **CRM Systems or Task Management**:
   - The output from an Airtable agent can be linked to nodes that push the data into a CRM (like HubSpot or Zoho) or a task management system, automating workflows like lead tracking or project management.

7. **Email or Messaging Tools**:
   - After retrieving or updating data in Airtable, the output can trigger actions such as sending an email (using a service like SendGrid) or posting a message to a chat platform (like Slack or Discord), depending on your workflow.

### Examples of Output Connections
- **Retrieve a record from Airtable**: The retrieved data could be displayed to the user or sent to another tool for processing.
- **Add a new record to Airtable**: After adding, you might trigger a message to be sent confirming the successful addition or use the data elsewhere in the flow.
- **Update a record in Airtable**: You could connect this to a custom tool that performs an action based on the updated information.

In summary, the Airtable Agent can connect its output to various nodes such as display/UI, custom tools, logic branches, other databases, webhooks, CRMs, messaging systems, or API integrations depending on the needs of your workflow.

{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](../../../contributing/) to get started.
{% endhint %}
