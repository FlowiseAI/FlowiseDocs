# Tree Summarize

When provided with text chunks and a query, recursively build a tree structure and return the root node as the result.

**Pros**: Beneficial for summarization tasks

**Cons**: Accuracy of answer might be lost during traversal of tree structure

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

**Prompt**

```
Context information from multiple sources is below.
---------------------
{context}
---------------------
Given the information from multiple sources and not prior knowledge, answer the query.
Query: {query}
Answer:
```
