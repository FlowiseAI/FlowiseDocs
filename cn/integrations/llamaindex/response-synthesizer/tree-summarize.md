# 树总结

当提供文本块和查询时，递归地构建树结构并返回根节点作为结果。

**优点**：有利于总结任务

**缺点**：在遍历树结构期间可能会丢失答案的准确性

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

**提示词**

```
Context information from multiple sources is below.
---------------------
{context}
---------------------
Given the information from multiple sources and not prior knowledge, answer the query.
Query: {query}
Answer:
```
