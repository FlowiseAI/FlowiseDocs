# Vectara QA Chain

A chain for performing question-answering tasks with Vectara.

<figure><img src="../../../.gitbook/assets/screely-1700662138252.png" alt=""><figcaption></figcaption></figure>

## Definitions

**A retrieval-based question-answering chain**, which integrates with a Vectara retrieval component and allows you to configure input parameters and perform question-answering tasks.

## Inputs

* [Vectara Store](../vector-stores/vectara.md)

## Parameters

| Name                   | Description                                                   |
| ---------------------- | ------------------------------------------------------------- |
| Summarizer Prompt Name | model to be used in generating the summary                    |
| Response Language      | desired language for the response                             |
| Max Summarized Results | number of top results to use in summarization (defaults to 7) |

## Outputs

| Name           | Description                   |
| -------------- | ----------------------------- |
| VectaraQAChain | Final node to return response |
