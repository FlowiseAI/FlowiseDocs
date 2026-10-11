---
description: 从 Confluence 文档加载数据
---

# 汇合

## 汇合

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="263"><figcaption><p>汇合节点</p></figcaption></figure>

## Confluence 文档加载器

Confluence 是 Atlassian 的企业 wiki 和协作平台。该模块提供从 Confluence 空间和页面加载和处理内容的功能。

该模块提供了一个复杂的 Confluence 文档加载器，可以：

* 从特定 Confluence 空间加载内容
* 支持云和服务器/数据中心部署
* 处理多种方法的认证
* 限制检索的页数
* 使用文本分割器处理内容
* 自定义元数据提取

### 输入

#### 必需参数

* **基础 URL**：Confluence 实例 URL（例如 https://example.atlassian.net/wiki）
* **Space Key**：Confluence 空间的唯一标识符
* **连接凭据**：选择：
  * Confluence Cloud API 凭据（用户名 + 访问令牌）
  * Confluence 服务器/DC API 凭据（个人访问令牌）

#### 可选参数

* **文本分割器**：处理提取内容的文本分割器
* **限制**：检索的最大页数（0表示无限制）
* **附加元数据**：带有附加元数据的 JSON 对象
* **省略元数据键**：要省略的元数据键的逗号分隔列表

### 输出

* **Document**：包含元数据和页面内容的文档对象数组
* **文本**：来自文档页面内容的串联字符串

### 特点

* 多部署支持（云/服务器/DC）
* 灵活的身份验证选项
* 页数限制控制
* 内容处理能力
* 元数据定制
* 错误处理
* 支持文本分割

### 身份验证方法

#### 汇流云

* 需要用户名和访问令牌
* 从 Atlassian 帐户设置生成的访问令牌
* 支持API令牌认证

#### Confluence 服务器/数据中心

* 使用个人访问令牌
* 从 Confluence 实例生成的令牌
* 支持直接服务器访问

### 注释

* 空格键可以在 Confluence 空间设置中找到
* 云与服务器的不同身份验证方法
* 速率限制可能根据实例而适用
* 内容包括页面文本和元数据
* 支持完整和部分内容检索
* 无效凭据或 URL 的错误处理

### 寻找空格键

要查找您的 Confluence Space 密钥：

1. 导航至 Confluence 中的空间
2.进入空间设置
3.在概览中查找“空格键”
4. 格式示例：\~EXAMPLE362906de5d343d49dcdbae5dEXAMPLE
