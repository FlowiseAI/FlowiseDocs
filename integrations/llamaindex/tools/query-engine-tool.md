# Query Engine Tool

Turns Query Engine into a Tool which can then be used by [Sub-Question Query Engine](../engine/sub-question-query-engine.md) or Agent.

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

## Inputs

* Vector Store Index

## Parameters

| Name             | Description                                         |
| ---------------- | --------------------------------------------------- |
| Tool Name        | Name of the tool                                    |
| Tool Description | A description to tell when LLM should use this tool |

## Outputs

| Name            | Description                                                                                      |
| --------------- | ------------------------------------------------------------------------------------------------ |
| QueryEngineTool | Connecting point to Agent or [Sub-Question Query Engine](../engine/sub-question-query-engine.md) |
