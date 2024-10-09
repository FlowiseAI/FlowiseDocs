---
description: Agent used to answer queries on CSV data.
---

# CSV Agent

<figure><img src="../../../.gitbook/assets/image (16).png" alt="" width="273"><figcaption><p>CSV Agent Node</p></figcaption></figure>

## CSV Agent Functionality

The CSV Agent in FlowWise AI is designed to facilitate the extraction and analysis of data from CSV files. By utilizing this agent, users can upload a CSV file and query the data in a conversational manner, allowing for quick and efficient exploration of the dataset without needing to manually search through the file or write custom scripts.

For example, the CSV Agent can be used for:

- Extracting specific rows or columns based on a query.
- Summarizing data, such as calculating totals or averages for specific fields.
- Filtering the dataset based on conditions, such as retrieving records that meet certain criteria.

This functionality helps users get insights from their CSV data easily, without the need for advanced technical knowledge or direct access to data analysis tools.

## Inputs

The CSV Agent requires the following inputs to function effectively:

- **Language Model**: The language model to be used for processing queries and generating responses. This input is required to ensure the quality of the generated content and decision-making.
- **Input Moderation**: Optional input that enables content moderation. This helps ensure that queries are appropriate and do not contain offensive or harmful content.
- **CSV File**: A CSV file must be uploaded for the agent to process. This is a required input and forms the basis for the agent's responses. Users can upload their CSV file directly using the **Upload File** option.
- **Additional Parameters**: Optional parameters that can be used to customize the behavior of the agent. These parameters include:
  - **System Message**: A message that sets the behavior or persona of the CSV Agent. This can help shape how the agent interacts with the data and provides responses.
  - **Custom Pandas Read_CSV Code**: Users can specify custom code to be used when reading the CSV file. This allows for additional customization, such as handling specific file formats or preprocessing the data before querying. For more information, see [Custom Pandas read_csv function](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html).

## Output

The output from the CSV Agent is provided through the **AgentExecutor**, which handles the execution of queries against the CSV data and returns relevant information. The output can include filtered rows, summaries, or specific data points extracted from the CSV file.

The output from the CSV Agent can also be connected to other modules within FlowWise to enhance the overall workflow. Examples of possible connections include:

- **Conversational Agent**: The output can be passed to a Conversational Agent to provide users with an explanation or summary of the data.
- **Airtable Agent**: The data extracted can be used to update records in an Airtable base, making it possible to seamlessly integrate CSV-based information into existing databases.
- **Notification Agent**: If certain criteria are met within the CSV data (e.g., a threshold value is exceeded), the results can be connected to a Notification Agent to alert stakeholders via email or other channels.

These integrations help create seamless, end-to-end workflows that make it easier for users to derive value from their CSV data in an automated and interactive way.


{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](../../../contributing/) to get started.
{% endhint %}
