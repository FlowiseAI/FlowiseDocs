---
description: LangChain记录管理器节点
---

# 记录管理器

***

记录管理器会跟踪您的索引文档，防止在[矢量存储](vector-stores/)中出现重复的矢量嵌入。

当写入更新文档块时，将使用 [SHA-1](https://github.com/emn178/js-sha1) 算法对每个块进行哈希处理。这些哈希值将存储在记录管理器中。如果存在现有哈希，则将跳过嵌入和写入更新过程。

在某些情况下，您可能希望删除与正在编制索引的新文档具有相同来源的现有文档。为此，记录管理器有 3 种清理模式：

{% tabs %}
{% tab title="Incremental" %}
当您写入更新多个文档，并且想要防止删除不属于当前写入更新过程的现有文档时，请使用 **增量** 清理模式。

1. 让我们有一个记录管理器，其中 `Incremental` Cleanup 和 `source` 作为 SourceId 键

<div align="left"><figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="264"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="410"><figcaption></figcaption></figure></div>

2.并具备以下2份文件：

|文字|元数据 |
| ---- | ---------------- |
|猫 | `{source:"cat"}` |
|狗 | `{source:"dog"}` |

<div align="left"><figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="202"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (2).png" alt="" width="231"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. 写入更新后，我们将看到 2 个已更新的文档：

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2).png" alt="" width="433"><figcaption></figcaption></figure>

4. 现在，如果我们删除 **Dog** 文档，并将 **Cat** 更新为 **Cats**，我们现在将看到以下内容：

<figure><img src="../../.gitbook/assets/image (13) (2) (2).png" alt="" width="425"><figcaption></figcaption></figure>

* 原**Cat**文档被删除
* 添加了带有 **Cats** 的新文档
* **Dog** 文档保持不变
* 向量存储 中剩余的向量嵌入是 **Cats** 和 **Dog**

<figure><img src="../../.gitbook/assets/image (15) (1) (1) (1) (1) (1) (1).png" alt="" width="448"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Full" %}
当您写入更新多个文档时，**完整**清理模式将自动删除不属于当前写入更新过程的任何矢量嵌入。

1. 让我们有一个具有 `Full` Cleanup 功能的记录管理器。对于完全清理模式，我们不需要 SourceId Key。

<div align="left"><figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="264"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (2).png" alt="" width="407"><figcaption></figcaption></figure></div>

2.并具备以下2份文件：

|文字|元数据 |
| ---- | ---------------- |
|猫 | `{source:"cat"}` |
|狗 | `{source:"dog"}` |

<div align="left"><figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="202"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (2).png" alt="" width="231"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. 写入更新后，我们将看到 2 个已更新的文档：

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2).png" alt="" width="433"><figcaption></figcaption></figure>

4. 现在，如果我们删除 **Dog** 文档，并将 **Cat** 更新为 **Cats**，我们现在将看到以下内容：

<figure><img src="../../.gitbook/assets/image (18) (1) (1) (1) (2).png" alt="" width="430"><figcaption></figcaption></figure>

* 原**Cat**文档被删除
* 添加了带有 **Cats** 的新文档
* **Dog** 文档已删除
* 向量存储 中剩余的向量嵌入只是 **Cats**

<figure><img src="../../.gitbook/assets/image (19) (1) (1) (1).png" alt="" width="527"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="None" %}
不会执行任何清理操作
{% endtab %}
{% endtabs %}

当前可用的记录管理器节点有：

* SQLite
* MySQL
* PostgreSQLSQL

## 资源

* [LangChain 索引 - 工作原理](https://js.langchain.com/docs/how_to/indexing/#how-it-works)
