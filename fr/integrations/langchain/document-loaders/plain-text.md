# Plain Text

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="263"><figcaption><p>Plain Text Node</p></figcaption></figure>

Plain text is the most basic form of text data, containing no formatting or other embedded information. This module provides functionality to load and process plain text content directly.

This module provides a straightforward text document loader that can:

* Load text content directly
* Process text with splitters
* Add custom metadata
* Handle escape characters
* Support document splitting
* Customize metadata extraction
* Manage text encoding

## Inputs

### Required Parameters

* **Text**: The plain text content to process

### Optional Parameters

* **Text Splitter**: A text splitter to process the content
* **Additional Metadata**: JSON object with additional metadata
* **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

* **Document**: Array of document objects containing metadata and pageContent
* **Text**: Concatenated string from pageContent of documents

## Features

* Direct text input
* Text splitting support
* Metadata handling
* Error handling
* Memory-efficient processing
* Character encoding handling
* Flexible output formats

## Text Processing

### Direct Mode

* Single document creation
* Preserves original text
* Basic metadata handling
* Memory efficient

### Split Mode

* Multiple document creation
* Custom splitting rules
* Individual chunk metadata
* Granular content access

## Document Structure

Each document contains:

* **pageContent**: Original or split text content
* **metadata**:
  * Custom metadata from input
  * Split-specific metadata (when using splitter)
  * Additional metadata properties

## Content Handling

### Text Input

* Direct string input
* Multi-line support
* Unicode support
* Escape character handling

### Processing Options

* Text splitting
* Metadata addition
* Character normalization
* Whitespace handling

## Notes

* Simple and efficient
* No file handling required
* Memory-efficient processing
* Error handling for invalid inputs
* Support for large texts
* Flexible output formats
* Metadata customization
* Character encoding support
