---
description: 从 API 加载数据。
---

# API 文档加载器

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1).png" alt="" width="273"><figcaption><p>API 加载器节点</p></figcaption></figure>

API 文档加载器提供使用 HTTP 请求加载和处理来自外部 API 的数据的功能。该模块支持与 RESTful API 和 Web 服务的无缝集成。

该模块提供了一个多功能的 API 文档加载器，可以：
- 发出 HTTP GET 和 POST 请求
- 处理自定义标头和请求正文
- 将 API 回复处理成文档
- 支持JSON数据结构
- 自定义元数据提取
- 使用文本分割器处理响应

## 输入

### 必需参数
- **URL**：要调用的 API 端点 URL
- **方法**：要使用的 HTTP 方法（GET 或 POST）

### 可选参数
- **标头**：包含 HTTP 标头的 JSON 对象
- **Body**：JSON 对象用于 POST 请求正文
- **文本分割器**：处理提取内容的文本分割器
- **附加元数据**：带有附加元数据的 JSON 对象
- **省略元数据键**：要省略的元数据键的逗号分隔列表

## 输出

- **Document**：包含元数据和页面内容的文档对象数组
- **文本**：来自文档页面内容的串联字符串

## 特点
- HTTP 方法支持 (GET/POST)
- 自定义标头配置
- 请求主体定制
- 响应处理
- 错误处理
- 元数据定制
- 文本分割功能

## 用法示例

### GET 请求
```json
{
    "method": "GET",
    "url": "https://api.example.com/data",
    "headers": {
        "Authorization": "Bearer token123",
        "Accept": "application/json"
    }
}
```

### POST 请求
```json
{
    "method": "POST",
    "url": "https://api.example.com/data",
    "headers": {
        "Content-Type": "application/json",
        "Authorization": "Bearer token123"
    },
    "body": {
        "query": "example",
        "limit": 10
    }
}
```

## 注释
- 支持 JSON 请求/响应格式
- 处理 HTTP 错误响应
- 自动将响应数据处理成文档
- 可以与文本分割器结合进行内容处理
- 支持自定义元数据添加和省略
- 错误响应得到正确处理和报告
