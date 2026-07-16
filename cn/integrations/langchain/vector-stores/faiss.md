---
description: >-
  使用 Meta 的 Faiss 库写入更新嵌入数据并执行相似性搜索。
---

#费斯

Faiss 是一个用于高效相似性搜索和密集向量聚类的库。与云矢量存储不同，Faiss 在您的计算机上本地运行并将数据保存到您的文件系统中。

<figure><img src="../../../.gitbook/assets/image (158).png" alt="" width="307"><figcaption><p>费斯节点</p></figcaption></figure>

## 先决条件

### 方法一：NPM/源码安装
对于本地安装，请确保您的计算机可以编译 C++ 模块。
* **Linux：** 安装 `build-essential`。
* **MacOS：** 运行 `xcode-select --install` 以获取 Clang/C++ 工具。
* **Windows：** 安装 Visual Studio 构建工具和 Python。

### 方法2：Docker
如果您使用 Docker 运行 Flowise，则可以使用 Faiss。

## 设置

### 输入

|输入 |描述 |
| :--- | :--- |
| **文件** |连接文档加载器类别中的任何节点。 |
| **嵌入** |连接嵌入类别中的任何节点。 |

### 参数

* **基本路径：** 将保存索引文件（`faiss.index` 和 `docstore.json`）的文件夹路径。
    * 如果留空，数据将存储在 RAM 中，并会在 Flowise 重新启动时丢失。
    * **Docker 路径示例：** `/root/.flowise/faiss`
    * > **注意：** 确保路径映射到 `docker-compose.yml` 文件中的卷，以防止容器重新启动时数据丢失。
    * **本地路径示例：** `C:\Users\YourName\flowise_data`

## 配置和摄取

1. 在画布上添加一个新的 **Faiss** 节点。
2. 在 **Faiss** 节点的 **Base Path** 字段中输入文件夹路径。
3. 将文档加载器和嵌入节点连接到 **Faiss** 节点。
4. 单击 **写入更新矢量数据库** 处理您的文档。

## 验证

要验证您的数据是否已写入更新，请导航到您在 **基本路径** 字段中指定的文件夹。您应该看到文件 `faiss.index` 和 `docstore.json`。

## 资源
* [LangChain JS Faiss文档](https://docs.langchain.com/oss/javascript/integrations/vectorstores/faiss)
