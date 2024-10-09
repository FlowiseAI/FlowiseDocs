---
description: An agent that uses OpenAI Assistant API to pick the tool and args to call.
---

# OpenAI Assistant

<figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="272"><figcaption><p>OpenAI Assistant</p></figcaption></figure>
## OpenAI Assistant Functionality

The OpenAI Assistant in FlowWise AI is designed to provide versatile conversational capabilities by allowing users to select or create assistants powered by OpenAI models. Users can leverage pre-defined assistants or create new ones tailored to their specific requirements, making it easy to customize conversational AI to different contexts, from customer support to personal productivity.

For example, the OpenAI Assistant can be used for:

- Answering questions and providing detailed information based on a specific assistant persona.
- Engaging in interactive dialogues to support learning or problem-solving.
- Acting as a personal assistant for scheduling, reminders, or general inquiries.

The flexibility to create or select an assistant allows users to adapt this agent for a wide range of use cases, enhancing its utility and relevance in various workflows.

## Inputs

The OpenAI Assistant requires the following inputs to function effectively:

- **OpenAI Credential**: The credential required to authenticate access to OpenAI's services. This input is required to ensure the assistant can interact with OpenAI models.
- **Assistant Model**: The specific model that the assistant will use to generate responses. This input is required to determine the quality and nature of responses provided by the assistant.
- **Allowed Tools**: A list of tools that the assistant can access during interactions. This input defines the assistant's capabilities, such as retrieving data, analyzing content, or integrating with other services.
- **Input Moderation**: Optional input that enables content moderation to ensure that user queries and assistant responses remain appropriate.
- **Select Assistant**: Users can select an existing assistant from a list or create a new one. Note that users can only select assistants that they have created under their own OpenAI account. The creation process includes specifying:
  - **Assistant Name**: An optional field to name the new assistant.
  - **Assistant Description**: A brief description of what the assistant does, providing context for its role.
  - **Assistant Icon Source**: Optional input to set a custom icon for the assistant, adding a visual identifier.
  - **Assistant Instruction**: A customizable instruction that defines the assistant's role and behavior, guiding how it responds to user prompts. For example: "You are a personal math tutor. When asked a question, write and run Python code to answer the question."
  - **Assistant Temperature**: A parameter to control the randomness of the assistant's responses. Higher values (e.g., `1`) make the output more random, while lower values (e.g., `0`) make it more deterministic.
  - **Assistant Top P**: A parameter that controls the diversity of the generated responses. It is used to limit the probability mass of the output, ensuring that only the most relevant completions are considered.

## Output

The output from the OpenAI Assistant is provided through the **OpenAIAssistant**, which processes user queries and generates relevant responses based on the selected assistant's persona and model.

The output from the OpenAI Assistant can be connected to various nodes within FlowWise, depending on how the flow is structured. Specifically, the OpenAI Assistant node takes input from tools and connects to subsequent nodes in FlowWise. These nodes can represent actions such as creating leads, adding data to a CRM, sending messages, or other specific outcomes depending on how the user configures the flow. The assistant outputs text, data, or actions that are passed along the flow, often using custom tools or function calls integrated into FlowWise.

Potential integrations include:

- **Display/Interface Nodes**: To return answers to users in a chat or web interface.
- **Custom Tools**: To trigger functions like adding leads to a CRM system, sending emails, or interacting with third-party APIs.
- **Function Calls**: If tools or APIs are integrated to manage tasks such as scheduling appointments, saving data, or interacting with external systems like databases.

These integrations enable the OpenAI Assistant to serve as an adaptable, interactive interface that connects users to a wide range of AI-driven capabilities, enhancing both personal productivity and business workflows.

{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](../../../../contributing/) to get started.
{% endhint %}
