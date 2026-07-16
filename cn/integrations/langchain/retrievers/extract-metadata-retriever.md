# 提取元数据检索器

该检索器旨在自动从查询中提取关键字。提取的 JSON 输出用作向量存储的元数据过滤器。

例如，当我们问一个问题：“Apple 的利润是多少”时，LLM 将给出 `{source: "apple"}` 的输出，并且该输出将被传递到 Vectore Store 的元数据过滤器。

<figure><img src="../../../.gitbook/assets/image (5) (6).png" alt=""><figcaption></figcaption></figure>
