---
description: 从 JSON 文件加载数据。
---

# Json 文件

<figure><img src="../../../.gitbook/assets/image (12) (1) (1) (1) (2).png" alt="" width="259"><figcaption><p>Json 文件节点</p></figcaption></figure>

JSON（JavaScript 对象表示法）是一种轻量级数据交换格式，易于人类阅读和编写，易于机器解析和生成。该模块提供了在工作流程中加载和处理 JSON 文件的高级功能。

该模块提供了一个复杂的 JSON 文档加载器，可以：

* 加载单个或多个JSON 文件
* 支持base64编码的文件和存储中的文件
* 使用JSON指针提取特定数据
* 处理动态元数据提取
* 处理嵌套的 JSON 结构

## 输入

* **JSON 文件**：要处理的 JSON 文件（需要 .json 扩展名）
* **文本分割器**（可选）：用于处理提取内容的文本分割器
* **指针提取**（可选）：以逗号分隔的 JSON 指针列表，用于提取特定数据
* **附加元数据**（可选）：JSON 对象，用于从文档中动态提取元数据
* **省略元数据键**（可选）：要从默认元数据中省略的以逗号分隔的元数据键列表

## 输出

* **Document**：包含元数据和页面内容的文档对象数组
* **文本**：来自文档页面内容的串联字符串

## 特点

* 多文件处理支持
* JSON 基于指针的数据提取
* 动态元数据映射
* 嵌套JSON结构处理
* 存储集成支持
* Base64 和 blob 处理能力

## 用法示例

对于 JSON 文档，例如：

```json
[
    {
        "url": "https://www.google.com",
        "body": "This is body 1"
    },
    {
        "url": "https://www.yahoo.com",
        "body": "This is body 2"
    }
]
```

您可以使用以下方法提取特定字段作为元数据：

```json
{
    "source": "/url"
}
```

这会将 URL 值添加为元数据，并为每个文档添加键“source”。
