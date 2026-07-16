---
description: 从 Airtable 表加载数据。
---

# 空中桌

<figure><img src="../../../.gitbook/assets/image_airtable.png" alt="" width="271"><figcaption><p>空中桌节点</p></figcaption></figure>

Airtable 是一种云协作服务，它将电子表格的功能与数据库相结合。该模块提供了从 Airtable 表加载和处理数据的全面功能。

该模块提供了一个复杂的 Airtable 文档加载器，可以：

* 从特定的 Airtable 库、表和视图加载数据
* 过滤并选择特定字段
* 处理分页和大数据集
* 支持自定义公式过滤
* 使用文本分割器处理数据
* 自定义元数据提取

## 输入

### 必需参数

* **Base Id**：Airtable 基本标识符（例如 app11RobdGoX0YNsC）
* **Table Id**：特定的表标识符（例如，tblJdmvbrgizbYICO）
* **连接凭据**：Airtable API 凭据

### 可选参数

* **视图 ID**：特定视图标识符（例如 viw9UrP77Id0CE4ee）
* **文本分割器**：处理提取内容的文本分割器
* **仅包含字段**：要包含的字段名称或 ID 的逗号分隔列表
* **Return All**：是否返回所有结果（默认：true）
* **限制**：Return All 为 false 时返回的结果数（默认值：100）
* **按公式过滤**：用于过滤记录的 Airtable 公式
* **附加元数据**：带有附加元数据的 JSON 对象
* **省略元数据键**：要省略的元数据键的逗号分隔列表

## 输出

* **Document**：包含元数据和页面内容的文档对象数组
* **文本**：来自文档页面内容的串联字符串

## 特点

* 基于API的数据检索
* 字段选择和过滤
* 分页支持
* 基于公式的过滤
* 可定制的元数据处理
* 文本分割功能
* 无效输入的错误处理

## 注释

* 需要有效的 Airtable API 凭据
* 基础ID和表ID为必填项
* 包含逗号的字段名称应使用字段 ID 代替
* 过滤公式必须遵循 Airtable 公式语法
* 速率限制和 API 配额适用
* 支持完整和部分数据检索

## URL 结构示例

对于表 URL 例如：

```
https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYICO/viw9UrP77Id0CE4ee
```

* 基本 ID：app11RobdGoX0YNsC
*表ID：tblJdmvbrgizbYICO
*查看ID：viw9UrP77Id0CE4ee
