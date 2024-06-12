---
description: LangChain Chain Nodes
---

# Chains

***

In the context of chatbots and large language models, "chains" typically refer to sequences of text or conversation turns. These chains are used to store and manage the conversation history and context for the chatbot or language model. Chains help the model understand the ongoing conversation and provide coherent and contextually relevant responses.

Here's how chains work:

1. **Conversation History**: When a user interacts with a chatbot or language model, the conversation is often represented as a series of text messages or conversation turns. Each message from the user and the model is stored in chronological order to maintain the context of the conversation.
2. **Input and Output**: Each chain consists of both user input and model output. The user's input is usually referred to as the "input chain," while the model's responses are stored in the "output chain." This allows the model to refer back to previous messages in the conversation.
3. **Contextual Understanding**: By preserving the entire conversation history in these chains, the model can understand the context and refer to earlier messages to provide coherent and contextually relevant responses. This is crucial for maintaining a natural and meaningful conversation with users.
4. **Maximum Length**: Chains have a maximum length to manage memory usage and computational resources. When a chain becomes too long, older messages may be removed or truncated to make room for new messages. This can potentially lead to loss of context if important conversation details are removed.
5. **Continuation of Conversation**: In a real-time chatbot or language model interaction, the input chain is continually updated with the user's new messages, and the output chain is updated with the model's responses. This allows the model to keep track of the ongoing conversation and respond appropriately.

Chains are a fundamental concept in building and maintaining chatbot and language model conversations. They ensure that the model has access to the context it needs to generate meaningful and context-aware responses, making the interaction more engaging and useful for users.

### Chain Nodes:

* [GET API Chain](get-api-chain.md)
* [OpenAPI Chain](openapi-chain.md)
* [POST API Chain](post-api-chain.md)
* [Conversation Chain](conversation-chain.md)
* [Conversational Retrieval QA Chain](conversational-retrieval-qa-chain.md)
* [LLM Chain](llm-chain.md)
* [Multi Prompt Chain](multi-prompt-chain.md)
* [Multi Retrieval QA Chain](multi-retrieval-qa-chain.md)
* [Retrieval QA Chain](retrieval-qa-chain.md)
* [Sql Database Chain](sql-database-chain.md)
* [Vectara QA Chain](vectara-chain.md)
* [VectorDB QA Chain](vectordb-qa-chain.md)
