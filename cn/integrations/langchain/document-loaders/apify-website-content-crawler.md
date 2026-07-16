---
description: 从 Apify 网站内容爬虫加载数据。
---

# Apify 网站内容爬虫

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="266"><figcaption><p>Apify网站内容爬虫节点</p></figcaption></figure>

[Apify](https://apify.com/) 网站内容抓取工具是一款功能强大的网络抓取工具，可以使用各种抓取引擎从网站中提取内容。该模块提供与 Apify 网站内容爬虫的集成，以加载和处理 Web 内容。

该模块提供了一个复杂的网络爬虫，可以：

* 从指定的起始URL抓取多个网站
* 使用不同的抓取引擎（Chrome、Firefox、Cheerio、JSDOM）
* 控制抓取深度和页面限制
* 处理 JavaScript 渲染的内容
* 使用文本分割器处理提取的内容
* 自定义元数据提取

## 输入

### 必需参数

* **开始 URL**：以逗号分隔的 URL 列表，从中开始抓取
* **连接 Apify API**：Apify API 凭据
* **爬虫类型**：爬虫引擎的选择：
  * 无头网络浏览器（Chrome+Playwright）
  * 隐形网络浏览器（Firefox+Playwright）
  * 原始 HTTP 客户端 (Cheerio)
  * 具有 JavaScript 执行功能的原始 HTTP 客户端 (JSDOM)

### 可选参数

* **文本分割器**：处理提取内容的文本分割器
* **最大爬行深度**：要遵循的页面链接的最大深度（默认值：1）
* **Max Crawl Pages**：要抓取的最大页面数（默认值：3）
* **附加输入**：具有附加爬虫配置的 JSON 对象
* **附加元数据**：带有附加元数据的 JSON 对象
* **省略元数据键**：要省略的元数据键的逗号分隔列表

## 输出

* **Document**：包含元数据和页面内容的文档对象数组
* **文本**：来自文档页面内容的串联字符串

## 特点

* 多种抓取引擎支持
* 可配置爬取参数
* JavaScript渲染支持
* 深度和页数限制控制
* 元数据定制
* 文本分割功能
* 错误处理

## 爬虫类型

### 无头 Chrome（剧作家）

* 最适合现代网络应用程序
* 完整的 JavaScript 支持
* 更高的资源使用率

### 隐秘的火狐（剧作家）

* 适合有机器人检测的网站
* 完整的 JavaScript 支持
* 操作更加隐秘

### 谢里奥

* 快速且轻量
* 不支持 JavaScript
* 降低资源使用率

### JSDOM（实验）

* JavaScript执行支持
* 浏览器的轻量级替代品
* 实验特性

## 注释

* 需要有效的 Apify API 令牌
* 不同类型的爬虫有不同的能力
* 资源使用情况因爬虫类型而异
* JavaScript 支持取决于爬虫类型
* 根据 Apify 计划，速率限制可能适用
* 可通过 JSON 输入进行其他配置

## 抓取整个网站

1._（可选）_连接[**文本分割器**](../text-splitters/)。
2. 连接 Apify API（使用您的 [Apify API 令牌](https://my.apify.com/account#/integrations) 创建新凭据）。
3. 输入一个或多个爬虫将启动的 URL（以逗号分隔），例如 `https://docs.flowiseai.com/`。
4. 选择爬虫类型。有关详细信息，请参阅[网站内容爬网程序文档](https://apify.com/apify/website-content-crawler/input-schema#crawlerType)。
5._（可选）_指定其他参数，例如最大爬网深度和最大爬网页面数。

## 输出

将网站内容加载为文档。

## 资源

* [Apify-Flowise 集成](https://docs.apify.com/platform/integrations/flowise)
* [网站内容抓取工具](https://apify.com/apify/website-content-crawler)
