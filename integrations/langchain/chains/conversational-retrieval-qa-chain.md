# Conversational Retrieval QA Chain

A chain for performing question-answering tasks with a retrieval component.

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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
