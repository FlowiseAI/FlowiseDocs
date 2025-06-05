---
description: Use Unstructured.io to load data from a file path.
---

# Unstructured File Loader

<figure><img src="../../../.gitbook/assets/image (90).png" alt="" width="332"><figcaption><p>Unstructured File Loader Node</p></figcaption></figure>

The Unstructured File Loader uses [Unstructured.io](https://unstructured.io) to extract and process content from various file formats. It provides advanced document parsing capabilities with configurable options for OCR, chunking, and metadata extraction.

## Features
- Advanced document parsing
- OCR support with multiple language options
- Flexible chunking strategies
- Table structure inference
- Coordinate extraction
- Page break handling
- XML tag processing
- Customizable model selection
- Metadata extraction

## Configuration

### API Setup
- Default API URL: `https://api.unstructuredapp.io/general/v0/general`
- Requires API key from Unstructured.io
- Can be configured via environment variables:
  - `UNSTRUCTURED_API_URL`
  - `UNSTRUCTURED_API_KEY`

### Processing Strategies
- **Strategy**: Default is "hi_res"
  - Options include various processing strategies for different document types
- **Chunking Strategy**:
  - None (default)
  - by_title (chunks text based on titles)

## Parameters

### Required Parameters
- **File**: The document to process
- **API Key**: Unstructured.io API key (if not set via environment)

### Optional Parameters

#### OCR Options
- **OCR Languages**: Array of languages for OCR processing
- **Encoding**: Specify document encoding

#### Processing Options
- **Coordinates**: Extract element coordinates (true/false)
- **PDF Table Structure**: Infer table structure in PDFs (true/false)
- **XML Tags**: Keep XML tags in output (true/false)
- **Skip Table Types**: Array of table types to skip inference
- **Hi-Res Model**: Specify the high-resolution model name
- **Include Page Breaks**: Include page break information (true/false)

#### Text Chunking Options
- **Multi-page Sections**: Handle sections across pages (true/false)
- **Combine Under N Chars**: Combine elements under specified character count
- **New After N Chars**: Create new element after specified character count
- **Max Characters**: Maximum characters per element

## Output Structure

### Document Format
Each processed element becomes a document with:
- **pageContent**: Extracted text content
- **metadata**: 
  - category: Element type
  - Additional metadata from the processing

### Element Types
The loader can identify various element types:
- Text blocks
- Tables
- Lists
- Headers
- Footers
- Page breaks (if enabled)
- Other structural elements

## Usage Examples

### Basic Configuration
```typescript
{
  "apiKey": "your-api-key",
  "strategy": "hi_res",
  "ocrLanguages": ["eng"]
}
```

### Advanced Processing
```typescript
{
  "apiKey": "your-api-key",
  "strategy": "hi_res",
  "coordinates": true,
  "pdfInferTableStructure": true,
  "chunkingStrategy": "by_title",
  "multiPageSections": true,
  "combineUnderNChars": 100,
  "maxCharacters": 4000
}
```

## Notes
- API calls are made for each file processing request
- Response includes structured elements with text and metadata
- Elements are filtered to ensure valid text content
- Supports buffer-based processing
- Error handling for API responses
- Automatic metadata categorization
- Memory-efficient processing

## Best Practices
1. Set appropriate chunking parameters for your use case
2. Consider OCR language settings for non-English documents
3. Enable table structure inference for documents with tables
4. Use coordinates when spatial information is important
5. Configure character limits based on your downstream processing needs
6. Monitor API usage and response times
7. Handle potential API errors in your workflow

{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](broken-reference) to get started.
{% endhint %}
