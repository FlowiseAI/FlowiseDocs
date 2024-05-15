# Sub-Question Query Engine

A query engine designed to solve problem of answering a complex query using multiple data sources. It first breaks down the complex query into sub questions for each relevant data source, then gather all the intermediate reponses and synthesizes a final response.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

## Inputs

* Query Engine Tools
* Chat Model
* Embeddings
* [Response Synthesizer](../response-synthesizer/)

## Parameters

| Name                    | Description                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| Return Source Documents | To return citations/sources that were used to build up the response |

## Outputs

| Name                   | Description                   |
| ---------------------- | ----------------------------- |
| SubQuestionQueryEngine | Final node to return response |
