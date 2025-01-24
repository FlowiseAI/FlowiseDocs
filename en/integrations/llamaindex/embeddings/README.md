---
description: LlamaIndex Embeddings Nodes
---

# Embeddings

***

An embedding is a vector (list) of floating point numbers. The distance between two vectors measures their relatedness. Small distances suggest high relatedness and large distances suggest low relatedness.

Embeddings can be used to create a numerical representation of textual data. This numerical representation is useful because it can be used to find similar documents.

They are commonly used for:

* Search (where results are ranked by relevance to a query string)
* Clustering (where text strings are grouped by similarity)
* Recommendations (where items with related text strings are recommended)
* Anomaly detection (where outliers with little relatedness are identified)
* Diversity measurement (where similarity distributions are analyzed)
* Classification (where text strings are classified by their most similar label)

### Embedding Nodes:

* [Azure OpenAI Embeddings](azure-openai-embeddings.md)
* [OpenAI Embedding](openai-embedding.md)
