# 紧凑和精炼

当没有明确定义响应合成器时，这是默认值。

在每次 LLM 调用期间，通过填充尽可能多的适合最大提示词大小的文本块来压缩提示词。如果一个提示词中的内容太多，无法填充，请通过多个紧凑的提示词来“创建和完善”答案。

**优点**：与 [Refine](refine.md) 相同，适合提供更详细的答案，并且应该会减少 LLM 调用

**缺点**：由于多次 LLM 调用，可能会很昂贵

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

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
