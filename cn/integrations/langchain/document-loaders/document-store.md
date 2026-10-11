---
description: 从预先配置的文档存储加载数据。
---

# 文档存储

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="278"><figcaption><p>文档存储节点</p></figcaption></figure>

文档存储加载程序使您能够从数据库中预先配置的文档存储加载数据。该加载程序提供了一种便捷的方式来访问和利用工作流程中之前处理和存储的文档。

## 特点

* 从同步存储加载文档
* 自动元数据处理
* 多种输出格式
* 异步商店选择
* 数据库集成
* 基于块的文档检索
* JSON 元数据支持

## 它是如何工作的

1. **店铺选择**：
   * 列出所有处于“SYNC”状态的可用文档存储
   * 提供商店信息，包括名称和描述
   * 只允许从同步商店中选择
2. **文献检索**：
   * 从选定的存储中获取文档块
   * 使用原始元数据重建文档
   * 维护文档结构和关系

## 参数

### 必需参数

* **选择存储**：从可用的同步文档存储中选择
  * 显示商店名称和描述
  * 仅显示处于“SYNC”状态的商店
  * 根据数据库内容动态更新

## 输出

加载器提供两种输出格式：

### 文档输出

返回文档对象数组，每个对象包含：

* **pageContent**：文档块的实际内容
* **元数据**：JSON 格式的原始文档元数据

### 文本输出

返回一个连接字符串，其中包含：

* 所有文档块的内容
* 以换行符分隔
* 正确转义字符

## 数据库集成

加载程序通过以下方式与您的数据库集成：

* TypeORM数据源连接
* 文件存储实体管理
* 基于块的存储和检索
* 元数据保存

## 文档结构

每个加载的文档包含：

```typescript
{
  pageContent: string,    // The actual content
  metadata: {            // Parsed JSON metadata
    // Original document metadata
    // Store-specific information
    // Custom metadata fields
  }
}
```

## 用法示例

### 基本商店选择

```json
{
  "selectedStore": "store-id-123"
}
```

### 访问文档内容

```typescript
// Document output format
[
  {
    "pageContent": "Document content here...",
    "metadata": {
      "source": "original-file.pdf",
      "page": 1,
      "category": "reports"
    }
  }
]

// Text output format
"Document content here...\nNext document content here...\n"
```

## 最佳实践

1. 访问前确保存储同步
2. 为您的用例选择适当的输出格式
3. 在工作流程中适当处理元数据
4. 处理大文档时考虑块大小
5. 监控大型存储的数据库性能

## 注释

* 仅同步店铺可供选择
* 元数据自动从JSON解析
* 文档是从块重建的
* 支持文档和文本输出格式
* 与TypeORM集成以进行数据库访问
* 处理文本输出中的转义字符
* 保持原始文档结构

{% hint style="info" %}
本部分正在进行中。我们感谢您在完成本节时提供的任何帮助。请查看我们的[贡献指南](broken-reference/)以开始使用。
{% endhint %}
