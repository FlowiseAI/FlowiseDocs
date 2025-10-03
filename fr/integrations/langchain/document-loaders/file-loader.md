# File

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="282"><figcaption></figcaption></figure>

The File Loader is a versatile document loader that supports multiple file formats including TXT, JSON, CSV, DOCX, PDF, Excel, PowerPoint, and more. This module provides a unified interface for loading and processing various file types.

This module provides a sophisticated file loader that can:

* Load multiple file formats
* Support both base64-encoded files and files from storage
* Handle PDF-specific processing options
* Process JSON and JSONL with pointer extraction
* Support text splitting
* Customize metadata extraction
* Handle file storage integration

## Inputs

### Required Parameters

* **File**: The file(s) to process (supports multiple formats)

### Optional Parameters

* **Text Splitter**: A text splitter to process the extracted content
* **PDF Usage**: Choose between:
  * One document per page
  * One document per file
* **Use Legacy Build**: Use legacy build for PDF compatibility issues
* **JSONL Pointer Extraction**: Pointer name for JSONL files
* **Additional Metadata**: JSON object with additional metadata
* **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

* **Document**: Array of document objects containing metadata and pageContent
* **Text**: Concatenated string from pageContent of documents

## Supported File Types

* Text Files (.txt)
* JSON Files (.json)
* JSONL Files (.jsonl)
* CSV Files (.csv)
* PDF Files (.pdf)
* Word Documents (.docx)
* Excel Files (.xlsx, .xls)
* PowerPoint Files (.pptx, .ppt)
* And more...

## Features

* Multi-format support
* Storage integration
* PDF processing options
* JSON pointer extraction
* Text splitting support
* Metadata customization
* Error handling
* MIME type detection

## File Processing Options

### PDF Processing

* Per-page splitting
* Single document mode
* Legacy build support
* OCR compatibility

### JSON/JSONL Processing

* Pointer-based extraction
* Structured data handling
* Array processing
* Nested object support

## Notes

* Automatically detects file type
* Handles multiple files simultaneously
* Supports file storage integration
* Preserves file metadata
* Handles large files efficiently
* Error handling for invalid files
* Memory-efficient processing
