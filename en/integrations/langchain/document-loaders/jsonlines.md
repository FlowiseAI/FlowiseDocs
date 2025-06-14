# Json Lines File

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt="" width="256"><figcaption><p>Json Lines File Node</p></figcaption></figure>

JSON Lines (JSONL) is a text format where each line is a valid JSON value. This module provides functionality to load and process JSONL files, with support for pointer-based content extraction and dynamic metadata handling.

This module provides a sophisticated JSONL document loader that can:

* Load single or multiple JSONL files
* Extract specific values using JSON pointers
* Handle dynamic metadata extraction
* Process content with text splitters
* Support base64 encoded files
* Handle file storage integration
* Customize metadata extraction

## Inputs

### Required Parameters

* **JSONL File**: The JSONL file(s) to process (.jsonl extension)
* **Pointer Extraction**: JSON pointer to extract content (e.g., "key" for `{"key": "value"}`)

### Optional Parameters

* **Text Splitter**: A text splitter to process the extracted content
* **Additional Metadata**: JSON object with additional metadata
* **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

* **Document**: Array of document objects containing metadata and pageContent
* **Text**: Concatenated string from pageContent of documents

## Features

* JSON pointer extraction
* Dynamic metadata handling
* Text splitting support
* Base64 file support
* File storage integration
* Error handling
* Memory-efficient processing

## JSON Pointer Extraction

### Basic Example

For JSONL content:

```jsonl
{"key": "value1", "source": "file1.txt"}
{"key": "value2", "source": "file2.txt"}
```

With pointer "key", extracts: "value1", "value2"

### Dynamic Metadata

You can extract values as metadata using JSON pointers:

```json
{
    "source": "/source",
    "custom": "/metadata/field"
}
```

## Document Structure

Each document contains:

* **pageContent**: Extracted content using pointer
* **metadata**:
  * source: Original file path
  * line: Line number in file
  * pointer: Used JSON pointer
  * Additional dynamic metadata

## File Handling

### Local Files

* Direct file loading
* Base64 encoded content
* Multiple file support

### Storage Integration

* File storage system support
* Organization-based storage
* Chatflow-based storage

## Notes

* One document per JSONL line
* Invalid JSON lines are skipped
* Memory-efficient processing
* Error handling for invalid pointers
* Support for nested JSON structures
* Dynamic metadata extraction
* Flexible output formats
