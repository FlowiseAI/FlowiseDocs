# Context Chat Engine

A chat engine serves as an end-to-end pipeline for having a human-like conversation with your data, allowing for multiple exchanges rather than a single question-and-answer interaction.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Inputs

* Chat Model
* Vector Store Retriever
* [Memory](../../langchain/memory/)

## Parameters

| Name                    | Description                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| Return Source Documents | To return citations/sources that were used to build up the response |
| System Message          | An instruction for LLM on how to answer query                       |

## Outputs

| Name              | Description                   |
| ----------------- | ----------------------------- |
| ContextChatEngine | Final node to return response |
