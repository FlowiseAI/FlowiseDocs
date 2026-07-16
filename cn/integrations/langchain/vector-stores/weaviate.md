---
description: >-
  使用 Weaviate（a）写入更新嵌入数据并执行相似性或 mmr 搜索
  可扩展的开源矢量数据库。
---

# 维维特

<figure><img src="../../../.gitbook/assets/image (165).png" alt="" width="295"><figcaption><p>编织节点</p></figcaption></figure>

## 过滤

Weaviate 在过滤时支持以下[语法](https://weaviate.io/developers/weaviate/search/filters)：

**用户界面**

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (2).png" alt="" width="227"><figcaption></figcaption></figure>

**API**

```json
"overrideConfig": {
    "weaviateFilter": {
        "where": {
            "operator": "Equal",
            "path": [
                "test"
            ],
            "valueText": "key"
        }
    }
}
```

## 资源

* [LangchainJS Weaviate](https://js.langchain.com/v0.1/docs/integrations/vectorstores/weaviate/#usage-query-documents)
* [取消过滤](https://weaviate.io/developers/weaviate/search/filters)

{% hint style="info" %}
本部分正在进行中。我们感谢您在完成本节时提供的任何帮助。请查看我们的[贡献指南](/broken/pages/G48tdmpQ3z4CTWEspqkA)以开始使用。
{% endhint %}
