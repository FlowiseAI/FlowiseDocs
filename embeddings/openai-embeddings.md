# OpenAI Embeddings
**What are embeddings?** <br>

OpenAI’s text embeddings measure the relatedness of text strings. Embeddings are commonly used for: <br>
- Search (where results are ranked by relevance to a query string)
- Clustering (where text strings are grouped by similarity)
- Recommendations (where items with related text strings are recommended)
- Anomaly detection (where outliers with little relatedness are identified)
- Diversity measurement (where similarity distributions are analyzed)
- Classification (where text strings are classified by their most similar label)

An embedding is a vector (list) of floating point numbers. The distance between two vectors measures their relatedness. Small distances suggest high relatedness and large distances suggest low relatedness.
## Inputs 
Connect Credential = OpenAI API Key 
## Additional Parameters 
1. Strip new Lines
1. Batch Size
1. Timeout
1. Base Path
## Outputs
OpenAI Credential and Parameters
