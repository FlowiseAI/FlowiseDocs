# 带有文件加载器的文件夹

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="262"><figcaption><p>带有文件节点的文件夹</p></figcaption></figure>

文件夹加载器提供从目录加载和处理多个文件的功能。该模块支持多种文件格式，并且可以递归处理子目录。

该模块提供了一个复杂的文件夹加载器，可以：
- 同时加载多种文件类型
- 递归处理目录
- 处理各种文档格式
- 支持PDF特定处理
- 处理结构化数据文件
- 自定义元数据提取
- 支持文本分割

## 输入

### 必需参数
- **文件夹路径**：包含文件的目录的路径
- **递归**：是否处理子目录

### 可选参数
- **文本分割器**：处理提取内容的文本分割器
- **PDF 用法**：选择：
  - 每页一个文档
  - 每个文件一个文档
- **JSONL 指针提取**：JSONL 文件的指针名称
- **附加元数据**：带有附加元数据的 JSON 对象
- **省略元数据键**：要省略的元数据键的逗号分隔列表

## 输出

- **Document**：包含元数据和页面内容的文档对象数组
- **文本**：来自文档页面内容的串联字符串

## 支持的文件类型

### 文件
- PDF (.pdf)
- Word（.doc、.docx）
- Excel（.xls、.xlsx、.xlsm、.xlsb）
- PowerPoint（.ppt、.pptx）
- 文本（.txt）
- Markdown（.md、.markdown）
- HTML (.html)
- XML (.xml)

### 数据文件
- JSON (.json)
- JSONL (.jsonl)
- CSV (.csv)

### 编程语言
- Python（.py、.python）
- JavaScript (.js)
- 打字稿（.ts）
- Java (.java)
- C/C++（.c、.cpp、.h）
- C# (.cs)
- 红宝石（.rb、.ruby）
- 去（.go）
- PHP (.php)
- 斯威夫特（.swift）
- 铁锈 (.rs)
- Scala（.scala、.sc）
- 科特林 (.kt)
- 坚固性（.sol）

### 网络技术
- CSS (.css)
- SCSS (.scss)
- LESS (.less)
- SQL (.sql)
- 协议缓冲区（.proto）

## 特点
- 多格式支持
- 递归目录处理
- PDF 处理选项
- 结构化数据处理
- 文本分割支持
- 元数据定制
- 错误处理

## 注释
- 自动检测文件类型
- 处理大型目录
- 保留文件元数据
- 内存高效处理
- 支持自定义文件扩展名
- 无效文件的错误处理
- 灵活的输出格式