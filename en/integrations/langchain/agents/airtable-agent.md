---
description: Agent used to to answer queries on Airtable table.
---

# Airtable Agent

<figure><img src="../../../.gitbook/assets/image_airtable.png" alt="" width="271"><figcaption><p>Airtable Agent Node</p></figcaption></figure>

## Airtable Agent Functionality

The Airtable Agent is designed to facilitate interactions between Flowise AI and Airtable tables, enabling users to query Airtable data in a conversational manner. By using this agent, users can ask questions about the contents of their Airtable base and receive relevant responses based on the stored data. This can be particularly useful for quickly extracting specific pieces of information, automating workflows, or generating summaries from the data stored in Airtable.

For example, the Airtable Agent can be used to answer questions like:

* "How many tasks are still incomplete in my project tracker table?"
* "What are the contact details of the clients listed in the CRM?"
* "Give me a summary of all records added in the past week."

This functionality helps users get insights from their Airtable bases without needing to navigate through the Airtable interface, making it easier to manage and analyze their data in a seamless, interactive way.

## Inputs

The Airtable Agent requires the following inputs to function effectively:

* **Language Model**: The language model to be used for processing queries. This input is required and helps determine the quality and accuracy of responses provided by the agent.
* **Input Moderation**: Optional input that enables content moderation. This helps ensure that queries are appropriate and do not contain offensive or harmful content.
* **Connect Credential**: Required input to connect to Airtable. Users must select the appropriate credential that has permissions to access their Airtable data.
* **Base ID**: The ID of the Airtable base to connect to. This is a required field and can be found in the Airtable API documentation or the base settings. If your table URL looks like `https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYlCO/viw9UrP77idOCE4ee`, `app11RobdGoX0YNsC` is the Base ID. It is used to specify which Airtable base contains the data to be queried.
* **Table ID**: The ID of the specific table within the Airtable base. This is also a required field and helps the agent target the correct table for data retrieval. In the example URL `https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYlCO/viw9UrP77idOCE4ee`, `tblJdmvbrgizbYlCO` is the Table ID.
* **Additional Parameters**: Optional parameters that can be used to customize the behavior of the agent. These parameters can be configured based on specific use cases.
  * **Return All**: This option allows users to return all records from the specified table. If enabled, all records will be retrieved, otherwise, only a limited number will be returned.
  * **Limit**: Specifies the maximum number of records to be returned if **Return All** is not enabled. The default value is `100`.

**Note**: This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](../../../contributing/) to get started.
