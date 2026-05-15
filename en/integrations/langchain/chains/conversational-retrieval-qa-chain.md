# Conversational Retrieval QA Chain

A chain for performing question-answering tasks with a retrieval component.

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Definitions

**A retrieval-based question-answering chain**, which integrates with a retrieval component and allows you to configure input parameters and perform question-answering tasks.\
**Retrieval-Based Chatbots:** Retrieval-based chatbots are chatbots that generate responses by selecting pre-defined responses from a database or a set of possible responses. They "retrieve" the most appropriate response based on the input from the user.\
**QA (Question Answering):** QA systems are designed to answer questions posed in natural language. They typically involve understanding the question and searching for or generating an appropriate answer.

## Inputs

* [Language Model](../chat-models/)
* [Vector Store Retriever](../vector-stores/)
* [Memory (optional)](../memory/)

## Parameters

| Name                    | Description                                                                                                                                               |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Return Source Documents | To return citations/sources that were used to build up the response                                                                                       |
| System Message          | An instruction for LLM on how to answer query                                                                                                             |
| Chain Option            | Method on how to summarize, answer questions, and extract information from documents. Read [more](https://js.langchain.com/docs/modules/chains/document/) |

## Outputs

| Name                           | Description                   |
| ------------------------------ | ----------------------------- |
| ConversationalRetrievalQAChain | Final node to return response |


## Building a Simple RAG Pipeline

This example demonstrates a simple Retrieval-Augmented Generation (RAG) pipeline using a PDF document, OpenAI embeddings, an in-memory vector store, and a Conversational Retrieval QA Chain.

### Required Nodes

- PDF File Loader
- Recursive Character Text Splitter
- OpenAI Embeddings
- In-Memory Vector Store
- OpenAI Chat Model
- Conversational Retrieval QA Chain

### Node Connections

```text
Recursive Character Text Splitter → PDF File Loader

PDF File Loader → In-Memory Vector Store

OpenAI Embeddings → In-Memory Vector Store

In-Memory Vector Store → Conversational Retrieval QA Chain

OpenAI Chat Model → Conversational Retrieval QA Chain
```

### Explanation

The PDF File Loader loads the document content, while the Recursive Character Text Splitter breaks the document into smaller chunks for retrieval.

The OpenAI Embeddings node converts document chunks into vector representations. These embeddings are then stored in the In-Memory Vector Store together with the document chunks.

When a user asks a question, the Conversational Retrieval QA Chain retrieves the most relevant document chunks from the vector store and sends them to the chat model to generate a grounded response.

### Common Beginner Mistakes

- Connecting the embedding model directly to the Conversational Retrieval QA Chain
- Forgetting to connect the vector store retriever to the Conversational Retrieval QA Chain
- Confusing embeddings with chat models
- Forgetting to include required prompt variables such as `{context}` and `{question}`
- Connecting incorrect node output types together

### Recommended Prompt Rules

To reduce hallucinations and unsupported responses, consider adding rules such as:

- Use only the provided context to answer
- Do not use external knowledge
- If the information is not available in the provided context, clearly state that it was not found
- Avoid unsupported assumptions