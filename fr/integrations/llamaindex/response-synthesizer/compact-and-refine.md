# Compact And Refine

This is the default when no Response Synthesizer is explicilty defined.

Compact the prompt during each LLM call by stuffing as many text chunks that can fit within the maximum prompt size. If there are too many chunks to stuff in one prompt, "create and refine" an answer by going through multiple compact prompts.

**Pros**: The same as [Refine](refine.md), Good for more detailed answers, and should result in less LLM calls

**Cons**: Due to the multiple LLM calls , can be expensive

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

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
