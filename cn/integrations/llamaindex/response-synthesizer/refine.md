# 细化

通过按顺序浏览每个检索到的文本块来创建和完善答案。

**优点**：有利于获得更详细的答案

**缺点**：每个节点单独的 LLM 调用（可能很昂贵）

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

**优化提示词**

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

**文本质量检查提示词**

```
Context information is below.
---------------------
{context}
---------------------
Given the context information and not prior knowledge, answer the query.
Query: {query}
Answer:
```
