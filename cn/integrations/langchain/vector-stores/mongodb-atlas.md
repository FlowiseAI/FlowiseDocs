---
description: >-
  写入更新嵌入数据并在查询时执行相似性或 mmr 搜索
  MongoDB Atlas，一个托管云 mongodb 数据库。
---

# MongoDB 阿特拉斯

<figure><img src="../../../.gitbook/assets/image (161).png" alt="" width="308"><figcaption><p>MongoDB Atlas节点</p></figcaption></figure>

### 集群配置[​](https://js.langchain.com/docs/integrations/vectorstores/mongodb_atlas/#initial-cluster-configuration)<a href="#initial-cluster-configuration" id="initial-cluster-configuration"></a>

要设置 MongoDB Atlas 集群，请访问 [MongoDB Atlas](https://www.mongodb.com/) 网站并注册（如果您没有帐户）。出现提示词时，创建并命名您的集群，该集群将显示在“数据库”部分下。然后，选择“**浏览集合**”以创建新集合或使用提供的示例数据中的集合。

{% hint style="warning" %}
确保您创建的集群是7.0或更高版本。
{% endhint %}

### 创建索引

设置集群后，下一步是为要搜索的集合字段创建索引。

1. 转到 **Atlas 搜索** 选项卡，然后单击 **创建搜索索引**。
2. 选择 **Atlas Vector Search - JSON Editor**，选择适当的数据库和集合，然后将以下内容粘贴到文本框中：

```json
{
  "fields": [
    {
      "numDimensions": 1536,
      "path": "embedding",
      "similarity": "euclidean",
      "type": "vector"
    }
  ]
}
```

确保 `numDimensions` 属性对应于您正在使用的嵌入的维度。例如，Cohere 嵌入通常有 1024 个维度，而 OpenAI 嵌入默认有 1536 个维度。

**注意：** 矢量存储需要某些默认值，例如：

* 索引名称 `default`
* 集合字段名称 `embedding`
* 原始文本字段名称 `text`

确保使用与索引和集合架构匹配的字段名称初始化向量存储，如上面的示例所示。

完成此操作后，继续构建索引。

{% hint style="info" %}
本部分正在进行中。我们感谢您在完成本节时提供的任何帮助。请查看我们的[贡献指南](/broken/pages/G48tdmpQ3z4CTWEspqkA)以开始使用。
{% endhint %}

### Flowise 配置

拖放 MongoDB Atlas 向量存储，然后添加新凭据。使用 MongoDB Atlas 仪表板提供的连接字符串：

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

填写其余字段：

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt="" width="252"><figcaption></figcaption></figure>

您还可以从其他参数配置更多详细信息：

<figure><img src="../../../.gitbook/assets/image (164).png" alt="" width="518"><figcaption></figcaption></figure>
