---
description: Load data from an API.
---

# API Document Loader

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1).png" alt="" width="273"><figcaption><p>API Loader Node</p></figcaption></figure>

The API Document Loader provides functionality to load and process data from external APIs using HTTP requests. This module enables seamless integration with RESTful APIs and web services.

This module provides a versatile API document loader that can:
- Make HTTP GET and POST requests
- Handle custom headers and request bodies
- Process API responses into documents
- Support JSON data structures
- Customize metadata extraction
- Process responses with text splitters

## Inputs

### Required Parameters
- **URL**: The API endpoint URL to call
- **Method**: HTTP method to use (GET or POST)

### Optional Parameters
- **Headers**: JSON object containing HTTP headers
- **Body**: JSON object for POST request body
- **Text Splitter**: A text splitter to process the extracted content
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- HTTP method support (GET/POST)
- Custom header configuration
- Request body customization
- Response processing
- Error handling
- Metadata customization
- Text splitting capabilities

## Example Usage

### GET Request
```json
{
    "method": "GET",
    "url": "https://api.example.com/data",
    "headers": {
        "Authorization": "Bearer token123",
        "Accept": "application/json"
    }
}
```

### POST Request
```json
{
    "method": "POST",
    "url": "https://api.example.com/data",
    "headers": {
        "Content-Type": "application/json",
        "Authorization": "Bearer token123"
    },
    "body": {
        "query": "example",
        "limit": 10
    }
}
```

## Notes
- Supports JSON request/response formats
- Handles HTTP error responses
- Automatically processes response data into documents
- Can be combined with text splitters for content processing
- Supports custom metadata addition and omission
- Error responses are properly handled and reported
