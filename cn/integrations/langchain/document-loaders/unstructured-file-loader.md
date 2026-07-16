---
description: 使用 Unstructed.io 从文件路径加载数据。
---

# 非结构化文件加载器

<figure><img src="../../../.gitbook/assets/image (90).png" alt="" width="332"><figcaption><p>非结构化文件加载器节点</p></figcaption></figure>

非结构化文件加载器使用 [Unstructed.io](https://unstructured.io) 提取和处理各种文件格式的内容。它提供高级文档解析功能以及 OCR、分块和元数据提取的可配置选项。

## 特点
- 高级文档解析
- OCR 支持多种语言选项
- 灵活的分块策略
- 表结构推断
- 坐标提取
- 分页符处理
- XML 标签处理
- 可定制的型号选择
- 元数据提取

## 配置

### API 设置
- 默认 API URL: `https://api.unstructuredapp.io/general/v0/general`
- 需要 Unstructed.io 的 API 密钥
- 可以通过环境变量进行配置：
  - `UNSTRUCTURED_API_URL`
  - `UNSTRUCTURED_API_KEY`

### 处理策略
- **策略**：默认为“hi_res”
  - 选项包括针对不同文档类型的各种处理策略
- **分块策略**：
  - 无（默认）
  - by_title（基于标题的文本块）

## 参数

### 必需参数
- **文件**：要处理的文档
- **API 键**：Unstructed.io API 键（如果未通过环境设置）

### 可选参数

#### OCR 选项
- **OCR 语言**：用于 OCR 处理的语言数组
- **编码**：指定文档编码

#### 处理选项
- **坐标**：提取元素坐标（真/假）
- **PDF 表结构**：推断 PDF 中的表结构（正确/错误）
- **XML 标签**：在输出中保留 XML 标签（true/false）
- **跳过表类型**：要跳过推理的表类型数组
- **高分辨率模型**：指定高分辨率模型名称
- **包括分页符**：包括分页符信息（正确/错误）

#### 文本分块选项
- **多页部分**：处理跨页面的部分（正确/错误）
- **Combine Under N Chars**：合并指定字符数下的元素
- **New After N Chars**：在指定字符数后创建新元素
- **最大字符数**：每个元素的最大字符数

## 输出结构

### 文档格式
每个处理后的元素都会成为一个文档，其中包含：
- **pageContent**：提取的文本内容
- **元数据**： 
  - 类别：元素类型
  - 处理过程中的附加元数据

### 元素类型
加载器可以识别各种元素类型：
- 文本块
- 桌子
- 列表
- 标题
- 页脚
- 分页符（如果启用）
- 其他结构元件

## 用法示例

### 基本配置
```typescript
{
  "apiKey": "your-api-key",
  "strategy": "hi_res",
  "ocrLanguages": ["eng"]
}
```

### 先进处理
```typescript
{
  "apiKey": "your-api-key",
  "strategy": "hi_res",
  "coordinates": true,
  "pdfInferTableStructure": true,
  "chunkingStrategy": "by_title",
  "multiPageSections": true,
  "combineUnderNChars": 100,
  "maxCharacters": 4000
}
```

## 注释
- 对每个文件处理请求进行 API 调用
- 响应包括带有文本和元数据的结构化元素
- 元素被过滤以确保有效的文本内容
- 支持基于缓冲区的处理
- API 响应的错误处理
- 自动元数据分类
- 内存高效处理

## 最佳实践
1. 为您的用例设置适当的分块参数
2. 考虑非英语文档的 OCR 语言设置
3.为带有表格的文档启用表格结构推断
4. 当空间信息很重要时使用坐标
5. 根据下游处理需求配置字符限制
6. 监控 API 使用情况和响应时间
7. 处理工作流程中潜在的 API 错误

{% hint style="info" %}
本部分正在进行中。我们感谢您在完成本节时提供的任何帮助。请查看我们的[贡献指南](broken-reference)以开始使用。
{% endhint %}
