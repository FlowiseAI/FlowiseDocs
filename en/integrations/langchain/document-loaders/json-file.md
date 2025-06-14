---
description: Load data from JSON files.
---

# Json File

<figure><img src="../../../.gitbook/assets/image (12) (1) (1) (1) (2).png" alt="" width="259"><figcaption><p>Json File Node</p></figcaption></figure>

JSON (JavaScript Object Notation) is a lightweight data-interchange format that is easy for humans to read and write and easy for machines to parse and generate. This module provides advanced functionality to load and process JSON files within your workflow.

This module provides a sophisticated JSON document loader that can:

* Load single or multiple JSON files
* Support both base64-encoded files and files from storage
* Extract specific data using JSON pointers
* Handle dynamic metadata extraction
* Process nested JSON structures

## Inputs

* **JSON File**: The JSON file(s) to process (.json extension required)
* **Text Splitter** (optional): A text splitter to process the extracted content
* **Pointers Extraction** (optional): Comma-separated list of JSON pointers to extract specific data
* **Additional Metadata** (optional): JSON object for dynamic metadata extraction from the document
* **Omit Metadata Keys** (optional): Comma-separated list of metadata keys to omit from the default metadata

## Outputs

* **Document**: Array of document objects containing metadata and pageContent
* **Text**: Concatenated string from pageContent of documents

## Features

* Multiple file processing support
* JSON pointer-based data extraction
* Dynamic metadata mapping
* Nested JSON structure handling
* Storage integration support
* Base64 and blob handling capabilities

## Example Usage

For a JSON document like:

```json
[
    {
        "url": "https://www.google.com",
        "body": "This is body 1"
    },
    {
        "url": "https://www.yahoo.com",
        "body": "This is body 2"
    }
]
```

You can extract specific fields as metadata using:

```json
{
    "source": "/url"
}
```

This will add the URL value as metadata with key "source" for each document.
