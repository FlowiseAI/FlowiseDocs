# Simple Chat Engine

A simple chat engine functions as a complete pipeline for engaging in a dialogue between AI and user, without context retrieval. However it does equipped with [Memory](../../langchain/memory/), allowing to remember conversations.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

## Inputs

* Chat Model
* [Memory](../../langchain/memory/)

## Parameters

| Name           | Description                                   |
| -------------- | --------------------------------------------- |
| System Message | An instruction for LLM on how to answer query |

## Outputs

| Name             | Description                   |
| ---------------- | ----------------------------- |
| SimpleChatEngine | Final node to return response |
