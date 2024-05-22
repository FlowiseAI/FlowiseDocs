# Refine

Create and refine an answer by sequentially going through each retrieved text chunk.

**Pros**: Good for more detailed answers

**Cons**: Separate LLM call per Node (can be expensive)

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

**Refine Prompt**

```markup
The original query is as follows: {query}
We have provided an existing answer: {existingAnswer}
We have the opportunity to refine the existing answer (only if needed) with some more context below.
------------
{context}
------------
Given the new context, refine the original answer to better answer the query. If the context isn't useful, return the original answer.
Refined Answer:
```

**Text QA Prompt**

```
Context information is below.
---------------------
{context}
---------------------
Given the context information and not prior knowledge, answer the query.
Query: {query}
Answer:
```
