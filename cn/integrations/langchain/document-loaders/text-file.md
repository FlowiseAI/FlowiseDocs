---
description: 从文本文件加载数据。
---

# 文本文件

<figure><img src="../../../.gitbook/assets/image (89).png" alt="" width="322"><figcaption><p>文本文件节点</p></figcaption></figure>

文本文件加载器使您能够加载和处理各种基于文本的文件格式的内容。它支持多种文件类型，并为文本分割和元数据处理提供灵活的选项。

## 特点
- 支持多种基于文本的文件格式
- 多文件加载能力
- 文本分割支持
- 可定制的元数据处理
- 存储集成支持
- Base64 文件处理
- 多种输出格式

## 支持的文件类型
该加载程序支持多种基于文本的文件格式：
- 文本文件（.txt）
- 网页文件（.html、.aspx、.asp、.css）
- 编程语言：
  - C/C++（.cpp、.c、.h）
  - C# (.cs)
  - 去（.go）
  - Java (.java)
  - JavaScript/TypeScript（.js、.ts）
  - PHP (.php)
  - Python（.py、.python）
  - 红宝石（.rb、.ruby）
  - 铁锈 (.rs)
  - 斯卡拉（.sc，.scala）
  - 坚固性（.sol）
  - 斯威夫特（.swift）
  - Visual Basic (.vb)
- 标记/样式：
  - CSS/LESS/SCSS（.css、.less、.scss）
  - Markdown（.md、.markdown）
  - XML (.xml)
  - LaTeX（.tex、.ltx）
- 其他：
  - 协议缓冲区（.proto）
  - SQL (.sql)
  - RST (.rst)

## 输入

### 必需参数
- **Txt 文件**：要处理的一个或多个文本文件
  - 接受来自本地上传或存储的文件
  - 支持多文件选择

### 可选参数
- **文本分割器**：处理提取内容的文本分割器
- **附加元数据**：JSON 对象以及要添加到文档的附加元数据
- **省略元数据键**：要排除的元数据键的逗号分隔列表
  - 格式：`key1, key2, key3.nestedKey1`
  - 使用 * 删除所有默认元数据

## 输出

- **文档**：文档对象数组，包含：
  - 元数据：文件元数据和自定义字段
  - pageContent：提取的文本内容
- **文本**：所有提取内容的串联字符串

## 文档结构
每个文档包含：
- **pageContent**：文本文件的主要内容
- **元数据**：
  - 默认文件元数据
  - 其他自定义元数据（如果指定）
  - 过滤元数据（基于省略的键）

## 用法示例

### 单个文件处理
```json
{
  "txtFile": "example.txt",
  "metadata": {
    "source": "local",
    "category": "documentation"
  }
}
```

### 多个文件处理
```json
{
  "txtFile": ["doc1.txt", "doc2.md", "code.py"],
  "metadata": {
    "batch": "docs-2024",
    "processor": "text-loader"
  },
  "omitMetadataKeys": "source, timestamp"
}
```

## 存储集成
加载器支持两种文件源模式：
1. **直接上传**：通过界面直接上传文件
2. **存储集成**：通过存储系统访问文件
   - 格式：`FILE-STORAGE::filename.txt`
   - 支持组织和聊天流特定的存储

## 注释
- 处理单个和多个文件
- 支持base64编码的文件内容
- 自动处理不同的文件编码
- 大文件的内存高效处理
- 在需要时保留文件元数据
- 支持大文档的文本分割
- 处理输出文本中的转义字符
- 与组织特定的存储集成

{% hint style="info" %}
本部分正在进行中。我们感谢您在完成本节时提供的任何帮助。请查看我们的[贡献指南](broken-reference)以开始使用。
{% endhint %}
