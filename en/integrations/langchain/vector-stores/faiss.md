---
description: >-
  Upsert embedded data and perform similarity search using the Faiss library from Meta.
---

# Faiss

Faiss is a library for efficient similarity search and clustering of dense vectors. Unlike cloud vector stores, Faiss runs locally on your machine and saves data to your file system.

<figure><img src="../../../.gitbook/assets/image (158).png" alt="" width="307"><figcaption><p>Faiss Node</p></figcaption></figure>

## Prerequisites

### Method 1: NPM/Source Installation
For local installations, ensure your machine can compile C++ modules.
* **Linux:** Install `build-essential`.
* **MacOS:** Run `xcode-select --install` to get Clang/C++ tools.
* **Windows:** Install Visual Studio Build Tools and Python.

### Method 2: Docker
If you are running Flowise using Docker, Faiss is ready to use.

## Setup

### Inputs

| Input | Description |
| :--- | :--- |
| **Document** | Connect any node from the Document Loader category. |
| **Embeddings** | Connect any node from the Embeddings category. |

### Parameters

* **Base Path:** The folder path where the index files (`faiss.index` and `docstore.json`) will be saved.
    * If left blank, data is stored in RAM and will be lost when Flowise restarts.
    * **Docker Path Example:** `/root/.flowise/faiss`
    * > **Note:** Ensure the path is mapped to a volume in your `docker-compose.yml` file to prevent data loss upon container restart.
    * **Local Path Example:** `C:\Users\YourName\flowise_data`

## Configuration and Ingestion

1.  Add a new **Faiss** node on the canvas.
2.  Enter a folder path in the **Faiss** node’s **Base Path** field.
3.  Connect your Document Loader and Embeddings nodes to the **Faiss** node.
4.  Click **Upsert Vector Database** to process your documents.

## Verify

To verify your data has been upserted, navigate to the folder you specified in the **Base Path** field. You should see files `faiss.index` and `docstore.json`.

## Resources
* [LangChain JS Faiss Documentation](https://docs.langchain.com/oss/javascript/integrations/vectorstores/faiss)
