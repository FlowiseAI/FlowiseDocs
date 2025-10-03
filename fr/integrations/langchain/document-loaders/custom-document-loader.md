---
description: Custom function for loading documents.
---

# Custom Document Loader

<figure><img src="../../../.gitbook/assets/image_custom-loader (1).png" alt="" width="269"><figcaption><p>Custom Document Loader Node</p></figcaption></figure>

The Custom Document Loader provides the ability to create custom document loading functionality using JavaScript. This module enables flexible and customized document processing through user-defined functions.

This module provides a flexible document loader that can:
- Execute custom JavaScript functions for document loading
- Handle input variables dynamically
- Support both document and text outputs
- Run in a sandboxed environment
- Access flow context and variables
- Process custom metadata

## Inputs

### Required Parameters
- **Javascript Function**: Custom code that returns either:
  - Array of document objects (for Document output)
  - String (for Text output)

### Optional Parameters
- **Input Variables**: JSON object containing variables accessible in the function with $ prefix

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- Sandboxed execution environment
- Variable injection support
- Flow context access
- Custom dependency support
- Error handling
- Timeout protection
- Input validation

## Document Structure
When returning documents, each object must have:
```javascript
{
  pageContent: 'Document Content',
  metadata: {
    title: 'Document Title',
    // ... other metadata
  }
}
```

## Example Usage

### Document Output
```javascript
return [
  {
    pageContent: 'Document Content',
    metadata: {
      title: 'Document Title',
      source: 'Custom Source'
    }
  }
]
```

### Text Output
```javascript
return "Processed text content"
```

## Available Context
- **$input**: Input value passed to the function
- **$vars**: Access to flow variables
- **$flow**: Flow context object containing:
  - chatflowId
  - sessionId
  - chatId
  - input

## Notes
- Functions run in a secure sandbox
- 10-second execution timeout
- Built-in dependencies available
- External dependencies configurable
- Input variables must be valid JSON
- Error handling for invalid returns
- Supports async operations
