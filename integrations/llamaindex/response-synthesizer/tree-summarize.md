# Tree Summarize

Given a set of text chunks and the query, recursively construct a tree and return the root node as the response. Good for summarization purposes.

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

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
