# 谷歌云端硬盘

<figure><img src="../../../.gitbook/assets/image (282).png" alt="" width="317"><figcaption></figcaption></figure>

Google Drive 是一项云存储和文件同步服务。该模块提供从 Google Drive 加载和处理文件的功能，支持各种文件格式和 Google 工作区 文档。

该模块提供了一个复杂的 Google Drive 文档加载器，可以：

* 加载多种文件类型
* 处理 Google 工作区 文档
* 处理基于文件夹的加载
* 支持共享驱动器
* 递归处理文件
* 自定义文件类型过滤
* 处理OAuth2认证

### 必需参数

* **连接凭据**：Google Drive OAuth2 凭据。请参阅[#Google 云端硬盘](../tools/google-drive.md)
* **选择文件** 或 **文件夹 ID**：选择特定文件或提供文件夹 ID

### 可选参数

* **文件类型**：要加载的文件类型：
  * 谷歌文档
  * 谷歌表格
  * 谷歌幻灯片
  * PDF 文件
  * 文本文件
  * Word文档
  * 幻灯片
  * Excel 文件
* **包括子文件夹**：处理子文件夹中的文件
* **包括共享驱动器**：从共享驱动器访问文件
* **最大文件**：要加载的最大文件数（默认值：50）
* **文本分割器**：处理提取内容的文本分割器
* **附加元数据**：带有附加元数据的 JSON 对象
* **省略元数据键**：要省略的元数据键的逗号分隔列表

## 输出

* **Document**：包含元数据和页面内容的文档对象数组
* **文本**：来自文档页面内容的串联字符串

## 支持的文件类型

### Google 工作区

* 谷歌文档（application/vnd.google-apps.document）
* Google 表格 (application/vnd.google-apps.spreadsheet)
* 谷歌幻灯片（application/vnd.google-apps.presentation）

### 微软办公软件

* 字 (.docx)
* Excel (.xlsx)
* PowerPoint (.pptx)

### 其他格式

* PDF (.pdf)
* 文本文件 (.txt)

## 特点

* OAuth2认证
* 多种文件类型支持
* 文件夹处理
* 共享驱动器访问
* 文件类型过滤
* 支持文本分割
* 元数据定制
* 错误处理

## 加载方法

### 文件选择模式

* 直接选择文件
* 多文件支持
* 文件类型过滤
* 元数据保存

### 文件夹模式

* 递归文件夹处理
* 子文件夹支持
* 文件类型过滤
* 批处理

## 文档结构

每个文档包含：

* **pageContent**：从文件中提取的内容
* **元数据**：
  * fileName: 原始文件名
  * 文件类型：MIME 类型
  * fileId: Google 云端硬盘文件 ID
  * 来源：文件路径/URL
  * 额外的自定义元数据

## 注释

* 需要 OAuth2 身份验证
* 处理速率限制
* 支持大文件
* 临时文件管理
* 内存高效处理
* 无效文件的错误处理
* 自动令牌刷新
