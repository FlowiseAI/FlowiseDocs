---
description: 加载文档的自定义功能。
---

# 自定义文档加载器

<figure><img src="../../../.gitbook/assets/image_custom-loader (1).png" alt="" width="269"><figcaption><p>自定义文档加载器节点</p></figcaption></figure>

自定义文档加载器提供了使用 JavaScript 创建自定义文档加载功能的能力。该模块通过用户定义的功能实现灵活和定制的文档处理。

该模块提供了一个灵活的文档加载器，可以：
- 执行自定义 JavaScript 函数以加载文档
- 动态处理输入变量
- 支持文档和文本输出
- 在沙盒环境中运行
- 访问流程上下文和变量
- 处理自定义元数据

## 输入

### 必需参数
- **Javascript 函数**：返回以下任一内容的自定义代码：
  - 文档对象数组（用于文档输出）
  - 字符串（用于文本输出）

### 可选参数
- **输入变量**：JSON 对象，包含可在函数中使用 $ 前缀访问的变量

## 输出

- **Document**：包含元数据和页面内容的文档对象数组
- **文本**：来自文档页面内容的串联字符串

## 特点
- 沙盒执行环境
- 变量注入支持
- 流程上下文访问
- 自定义依赖支持
- 错误处理
- 超时保护
- 输入验证

## 文档结构
返回文档时，每个对象必须具有：
```javascript
{
  pageContent: 'Document Content',
  metadata: {
    title: 'Document Title',
    // ... other metadata
  }
}
```

## 用法示例

### 文档输出
```javascript
return [
  {
    pageContent: 'Document Content',
    metadata: {
      title: 'Document Title',
      source: 'Custom Source'
    }
  }
]
```

### 文本输出
```javascript
return "Processed text content"
```

## 可用上下文
- **$input**：传递给函数的输入值
- **$vars**：访问流变量
- **$flow**：流上下文对象包含：
  - 聊天流ID
  - 会话ID
  - 聊天ID
  - 输入

## 注释
- 函数在安全沙箱中运行
- 10秒执行超时
- 可用的内置依赖项
- 可配置的外部依赖项
- 输入变量必须有效 JSON
- 无效退货的错误处理
- 支持异步操作
