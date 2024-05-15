# Query Engine

A query engine serves as an end-to-end pipeline enabling users to ask questions about their data. It receives a natural language query and furnishes a response, accompanied by relevant context information retrieved and passed to the LLM (Large Language Model).

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

## Inputs

* Vector Store Retriever
* [Response Synthesizer](../response-synthesizer/)

## Parameters

| Name                    | Description                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| Return Source Documents | To return citations/sources that were used to build up the response |

## Outputs

| Name        | Description                   |
| ----------- | ----------------------------- |
| QueryEngine | Final node to return response |
