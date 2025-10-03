---
description: Load data from CSV files.
---

# CSV Files

<figure><img src="../../../.gitbook/assets/image_csv (1).png" alt="" width="271"><figcaption><p>Csv File Node</p></figcaption></figure>

CSV (Comma-Separated Values) is a simple file format used to store tabular data, such as a spreadsheet or database. This module provides functionality to load and process CSV files within your workflow.

This module provides a versatile CSV document loader that can:
- Load single or multiple CSV files
- Support both base64-encoded files and files from storage
- Extract specific columns or entire content
- Process large datasets efficiently
- Handle custom metadata management

## Inputs

- **CSV File**: The CSV file(s) to process (.csv extension required)
- **Text Splitter** (optional): A text splitter to process the extracted content
- **Single Column Extraction** (optional): Name of a specific column to extract
- **Additional Metadata** (optional): JSON object with additional metadata to add to documents
- **Omit Metadata Keys** (optional): Comma-separated list of metadata keys to omit from the default metadata

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of all documents

## Features
- Multiple file processing support
- Single column extraction capability
- Efficient handling of large datasets
- Customizable metadata handling
- Storage integration support
- Base64 and blob handling capabilities
