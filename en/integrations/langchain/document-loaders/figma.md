---
description: Load data from a Figma file.
---

# Figma Document Loader

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="264"><figcaption><p>Figma Node</p></figcaption></figure>

Figma is a collaborative web application for interface design. This module provides functionality to load and process content from Figma files, including text, components, and metadata.

This module provides a sophisticated Figma document loader that can:
- Load content from specific Figma files
- Extract text from selected nodes
- Process content recursively
- Handle authentication with Figma API
- Process content with text splitters
- Customize metadata extraction

## Inputs

### Required Parameters
- **File Key**: The unique identifier for the Figma file (from file URL)
- **Node IDs**: Comma-separated list of node identifiers to extract
- **Connect Credential**: Figma API credentials (access token)

### Optional Parameters
- **Recursive**: Whether to process nodes recursively
- **Text Splitter**: A text splitter to process the extracted content
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- API-based content extraction
- Node-level content selection
- Recursive processing
- Text splitting support
- Metadata customization
- Error handling
- Authentication management

## File Key Format
The file key can be found in the Figma file URL:
```
https://www.figma.com/file/:key/:title
```
Example: In `https://www.figma.com/file/12345/Website`, the file key is `12345`

## Node IDs
To get Node IDs:
1. Install the Node Inspector plugin in Figma
2. Select the desired elements
3. Copy the Node IDs from the inspector
4. Use comma-separated format: "0, 1, 2"

## Notes
- Requires valid Figma access token
- Node IDs must be valid for the file
- Supports recursive content extraction
- Can process multiple nodes at once
- Handles API rate limits and errors
- Preserves node hierarchy in metadata
- Supports custom metadata addition
