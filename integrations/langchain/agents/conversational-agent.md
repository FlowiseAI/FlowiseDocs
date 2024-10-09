---
description: Conversational agent for a chat model. It will utilize chat specific prompts.
---

# Conversational Agent

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1).png" alt="" width="271"><figcaption><p>Conversational Agent Node</p></figcaption></figure>

## Conversational Agent Functionality

The Conversational Agent in FlowWise AI is designed to facilitate natural language interactions with the AI, using a chat model to generate responses based on user input. By utilizing this agent, users can create conversational flows that are highly customizable, allowing for a wide range of use cases from customer support to automated personal assistants.

For example, the Conversational Agent can be used for:

- Answering frequently asked questions based on provided data.
- Engaging users in a dialogue to gather information or provide personalized suggestions.
- Serving as an AI-powered assistant that can understand and respond to user prompts in real-time.

This agent allows users to develop dynamic conversational experiences that can enhance customer engagement, automate routine tasks, and provide valuable insights through interactions.

## Inputs

The Conversational Agent requires the following inputs to function effectively:

- **Allowed Tools**: A list of tools that the agent can access during conversations. This input is required and defines the capabilities available to the agent, such as data retrieval, processing, or integrating with external APIs.
- **Chat Model**: The language model that will be used for generating responses during conversations. This is a required input and ensures that the Conversational Agent can understand and respond appropriately to user queries.
- **Memory**: Required input that allows the agent to remember past interactions during a conversation. This helps maintain context, enabling the agent to provide coherent responses based on the history of the dialogue.
- **Input Moderation**: Optional input to enable content moderation. This feature ensures that conversations remain appropriate and do not contain offensive or harmful content.
- **Additional Parameters**: These optional parameters can be used to customize the behavior of the Conversational Agent further:
  - **System Message**: A message that sets the initial behavior or persona of the agent, guiding how it interacts with users and the tone it should take.

## Output

The output from the Conversational Agent is provided through the **AgentExecutor**, which processes user inputs and generates relevant responses. The output can include text-based replies, actionable insights, or other forms of conversational feedback.

The output from the Conversational Agent can be connected to other FlowWise modules to build more advanced workflows. Examples of possible integrations include:

- **Notification Agent**: To send notifications or alerts based on information gathered during the conversation.
- **Airtable Agent**: If user inputs require database interactions, the output can be passed to the Airtable Agent to create, update, or retrieve records.
- **CSV Agent**: If the conversation involves data stored in CSV files, the output can be used to interact with the CSV Agent for data extraction or analysis.
- **OpenAI Tool Agent**: For performing specific actions that require a deeper integration with tools or APIs, the output from the conversation can be passed to an OpenAI Tool Agent to fulfill those tasks.

These connections make it possible to build robust, interactive systems where the Conversational Agent serves as the interface between users and a broader set of AI capabilities.

{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](../../../contributing/) to get started.
{% endhint %}
