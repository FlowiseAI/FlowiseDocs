---
description: Load data from DOCX files.
---

# Docx File

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="269"><figcaption><p>Docx File Node</p></figcaption></figure>

Microsoft Word Document (DOCX) is a widely used document format for creating and editing text documents. This module provides functionality to load and process DOCX files within your workflow.

This module provides a comprehensive DOCX document loader that can:

* Load single or multiple DOCX files
* Support both base64-encoded files and files from storage
* Extract text content with metadata
* Integrate with text splitters for content processing
* Handle custom metadata management

## Inputs

* **DOCX File**: The DOCX file(s) to process (.docx extension required)
* **Text Splitter** (optional): A text splitter to process the extracted content
* **Additional Metadata** (optional): JSON object with additional metadata to add to documents
* **Omit Metadata Keys** (optional): Comma-separated list of metadata keys to omit from the default metadata

## Outputs

* **Document**: Array of document objects containing metadata and pageContent
* **Text**: Concatenated string from pageContent of all documents

## Features

* Multiple file processing support
* Flexible text splitting options
* Customizable metadata handling
* Storage integration support
* Base64 and blob handling capabilities
