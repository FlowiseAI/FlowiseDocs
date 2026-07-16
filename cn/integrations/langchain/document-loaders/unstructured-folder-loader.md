---
description: >-
  使用 Unstructed.io 从文件夹加载数据。注：目前没有
  支持 .png 和 .heic，直到更新非结构化。
---

# 非结构化文件夹加载器

<figure><img src="../../../.gitbook/assets/image (101).png" alt="" width="320"><figcaption><p>非结构化文件夹加载器节点</p></figcaption></figure>

非结构化文件夹加载程序使用 [Unstructed.io](https://unstructured.io) 从文件夹加载和处理多个文档。它提供高级文档解析功能以及用于 OCR、分块和元数据提取的广泛配置选项。

{% hint style="warning" %}
目前不支持 .png 和 .heic 文件，直到更新非结构化文件。
{% endhint %}

## 特点
- 批量处理多个文档
- 多种处理策略
- OCR 支持超过 15 种语言
- 灵活的分块策略
- 表结构推断
- XML 处理选项
- 分页符处理
- 坐标提取
- 元数据定制

## 配置

### API 设置
- 默认 API URL: `http://localhost:8000/general/v0/general`
- 可以通过环境变量配置：`UNSTRUCTURED_API_URL`
- 可选的 API 密钥身份验证

## 参数

### 必需参数
- **文件夹路径**：包含要处理的文档的文件夹的路径

### 可选参数

#### 基本配置
- **非结构化 API URL**：API 端点（默认值：http://localhost:8000/general/v0/general）
- **策略**：处理策略（默认：自动）
  - hi_res：高分辨率处理
  - 快速：快速处理
  - ocr_only：以 OCR 为重点的处理
  - 自动：自动选择
- **Encoding**：文档编码（默认：utf-8）

#### OCR 选项
- **OCR 语言**：多语言支持包括：
  - 英语（eng）
  - 西班牙语（水疗）
  - 普通话 (cmn)
  - 印地语（hin）
  - 阿拉伯语（ara）
  - 葡萄牙语（por）
  - 孟加拉语（ben）
  - 俄语（rus）
  - 日语 (jpn)
  - 还有更多...

#### 处理选项
- **跳过推断表类型**：跳过表提取的文件类型（默认值：[“pdf”、“jpg”、“png”]）
- **高分辨率模型名称**：高分辨率策略的模型选择（默认： detectorron2_onnx）
  - 削片机：Unstructured 的内部 VDU 模型
  - detectorron2_onnx：Facebook AI 的快速物体检测
  - yolox：单级实时检测器
  - yolox_quantized：优化的YOLOX版本
- **坐标**：提取元素坐标（默认值：false）
- **包括分页符**：包括分页符元素
- **XML 保留标签**：保留 XML 标签
- **多页部分**：处理多页部分

#### 文本分块选项
- **分块策略**：文本分块方法（默认：by_title）
  - 无：无分块
  - by_title：按文档标题划分的块
- **在 N 个字符下合并**：最小块大小
- **新的 After N Chars**：软最大块大小
- **最大字符数**：硬最大块大小（默认值：500）

#### 元数据选项
- **Source ID Key**：文档来源标识的密钥（默认：来源）
- **附加元数据**：自定义元数据为 JSON
- **省略元数据键**：要从元数据中排除的键

## 支持的文件类型
- 文档：.doc、.docx、.odt、.ppt、.pptx、.pdf
- 电子表格：.xls、.xlsx
- 文本：.txt、.text、.md、.rtf
- 网页：.html、.htm
- 电子邮件：.eml、.msg
- 图片：.jpg、.jpeg（注意：目前不支持 .png 和 .heic）

## 输出结构

### 文档格式
每个处理的文档包括：
- **pageContent**：提取的文本内容
- **元数据**： 
  - 来源：文档来源标识符
  - 处理中的附加元数据
  - 自定义元数据（如果指定）

## 用法示例

### 基本配置
```json
{
  "folderPath": "/path/to/documents",
  "strategy": "auto",
  "encoding": "utf-8"
}
```

### 先进处理
```json
{
  "folderPath": "/path/to/documents",
  "strategy": "hi_res",
  "hiResModelName": "detectron2_onnx",
  "ocrLanguages": ["eng", "spa", "fra"],
  "chunkingStrategy": "by_title",
  "maxCharacters": 500,
  "coordinates": true,
  "metadata": {
    "source": "company_docs",
    "department": "legal"
  }
}
```

## 最佳实践
1.根据文档质量和处理需求选择合适的策略
2.根据文档内容配置OCR语言
3.调整分块参数以获得最佳文本分割
4. 针对您的用例使用适当的高分辨率模型
5.处理大文件夹时考虑内存使用
6. 监控 API 使用情况和响应时间
7. 处理工作流程中潜在的 API 错误

## 注释
- 批量处理多个文档
- 支持多种文件格式
- 内存高效处理
- 自动元数据处理
- 灵活的输出格式
- API 响应的错误处理
- 可配置的处理选项

{% hint style="info" %}
本部分正在进行中。我们感谢您在完成本节时提供的任何帮助。请查看我们的[贡献指南](broken-reference)以开始使用。
{% endhint %}
