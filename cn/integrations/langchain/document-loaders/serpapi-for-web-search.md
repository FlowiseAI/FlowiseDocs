---
description: 加载并处理网络搜索结果中的数据。
---

# 用于网页搜索的 SerpApi

<figure><img src="../../../.gitbook/assets/image (81).png" alt="" width="319"><figcaption><p>用于 Web 搜索节点的 SerpApi</p></figcaption></figure>

用于 Web 搜索的 SerpApi 加载程序使您能够使用 SerpApi 服务获取和处理 Web 搜索结果。该加载程序将搜索结果转换为结构化文档，可以轻松集成到您的工作流程中，使其成为需要实时网络搜索数据的应用程序的理想选择。

## 特点
- 实时网络搜索结果
- 文本分割功能
- 可定制的元数据处理
- 多种输出格式
- API 密钥认证
- 高效的文档处理

## 输入

### 必需参数
- **连接凭据**：SerpApi API 密钥凭据
- **查询**：要执行的搜索查询

### 可选参数
- **文本分割器**：处理提取内容的文本分割器
- **附加元数据**：JSON 对象以及要添加到文档的附加元数据
- **省略元数据键**：要排除的元数据键的逗号分隔列表
  - 格式：`key1, key2, key3.nestedKey1`
  - 使用 * 删除除自定义元数据之外的所有默认元数据

## 输出

- **文档**：文档对象数组，包含：
  - 元数据：搜索结果元数据
  - pageContent：搜索结果内容
- **文本**：所有搜索结果内容的串联字符串

## 文档结构
每个文档包含：
- **pageContent**：搜索结果的主要内容
- **元数据**：
  - 默认搜索结果元数据
  - 自定义元数据（如果指定）
  - 过滤元数据（基于省略的键）

## 元数据处理
自定义元数据的两种方法：
1. **附加元数据**
   - 通过 JSON 添加新的元数据字段
   - 与现有元数据合并
   - 对于添加自定义跟踪或分类很有用

2. **省略元数据键**
   - 删除不需要的元数据字段
   - 要排除的以逗号分隔的键列表
   - 支持嵌套键删除
   - 使用 * 删除所有默认元数据

## 使用提示词
- 提供特定的搜索查询以获得更好的结果
- 使用文本分割器获得大量搜索结果
- 自定义元数据以满足您的需求
- 进行多个查询时考虑速率限制
- 根据大小适当处理搜索结果

## 注释
- 需要 SerpApi API 密钥
- 遵守 API 速率限制
- 实时搜索结果
- 内存高效处理
- API 请求的错误处理
- 支持文档和文本输出格式

## 用法示例
```typescript
// Example search query
query: "artificial intelligence latest developments"

// Example additional metadata
metadata: {
  "source": "serpapi",
  "category": "tech",
  "timestamp": "2024-03-21"
}

// Example metadata keys to omit
omitMetadataKeys: "snippet, position, link"
```
