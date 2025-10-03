---
description: >-
  Use Unstructured.io to load data from a folder. Note: Currently doesn't
  support .png and .heic until unstructured is updated.
---

# Unstructured Folder Loader

<figure><img src="../../../.gitbook/assets/image (101).png" alt="" width="320"><figcaption><p>Unstructured Folder Loader Node</p></figcaption></figure>

The Unstructured Folder Loader uses [Unstructured.io](https://unstructured.io) to load and process multiple documents from a folder. It provides advanced document parsing capabilities with extensive configuration options for OCR, chunking, and metadata extraction.

{% hint style="warning" %}
Currently doesn't support .png and .heic files until unstructured is updated.
{% endhint %}

## Features
- Batch processing of multiple documents
- Multiple processing strategies
- OCR support with 15+ languages
- Flexible chunking strategies
- Table structure inference
- XML processing options
- Page break handling
- Coordinate extraction
- Metadata customization

## Configuration

### API Setup
- Default API URL: `http://localhost:8000/general/v0/general`
- Can be configured via environment variable: `UNSTRUCTURED_API_URL`
- Optional API key authentication

## Parameters

### Required Parameters
- **Folder Path**: Path to the folder containing documents to process

### Optional Parameters

#### Basic Configuration
- **Unstructured API URL**: API endpoint (default: http://localhost:8000/general/v0/general)
- **Strategy**: Processing strategy (default: auto)
  - hi_res: High resolution processing
  - fast: Quick processing
  - ocr_only: OCR-focused processing
  - auto: Automatic selection
- **Encoding**: Document encoding (default: utf-8)

#### OCR Options
- **OCR Languages**: Multiple language support including:
  - English (eng)
  - Spanish (spa)
  - Mandarin Chinese (cmn)
  - Hindi (hin)
  - Arabic (ara)
  - Portuguese (por)
  - Bengali (ben)
  - Russian (rus)
  - Japanese (jpn)
  - And more...

#### Processing Options
- **Skip Infer Table Types**: File types to skip table extraction (default: ["pdf", "jpg", "png"])
- **Hi-Res Model Name**: Model selection for hi_res strategy (default: detectron2_onnx)
  - chipper: Unstructured's in-house VDU model
  - detectron2_onnx: Facebook AI's fast object detection
  - yolox: Single-stage real-time detector
  - yolox_quantized: Optimized YOLOX version
- **Coordinates**: Extract element coordinates (default: false)
- **Include Page Breaks**: Include page break elements
- **XML Keep Tags**: Preserve XML tags
- **Multi-Page Sections**: Handle multi-page sections

#### Text Chunking Options
- **Chunking Strategy**: Text chunking method (default: by_title)
  - None: No chunking
  - by_title: Chunk by document titles
- **Combine Under N Chars**: Minimum chunk size
- **New After N Chars**: Soft maximum chunk size
- **Max Characters**: Hard maximum chunk size (default: 500)

#### Metadata Options
- **Source ID Key**: Key for document source identification (default: source)
- **Additional Metadata**: Custom metadata as JSON
- **Omit Metadata Keys**: Keys to exclude from metadata

## Supported File Types
- Documents: .doc, .docx, .odt, .ppt, .pptx, .pdf
- Spreadsheets: .xls, .xlsx
- Text: .txt, .text, .md, .rtf
- Web: .html, .htm
- Email: .eml, .msg
- Images: .jpg, .jpeg (Note: .png and .heic currently unsupported)

## Output Structure

### Document Format
Each processed document includes:
- **pageContent**: Extracted text content
- **metadata**: 
  - source: Document source identifier
  - Additional metadata from processing
  - Custom metadata (if specified)

## Usage Examples

### Basic Configuration
```json
{
  "folderPath": "/path/to/documents",
  "strategy": "auto",
  "encoding": "utf-8"
}
```

### Advanced Processing
```json
{
  "folderPath": "/path/to/documents",
  "strategy": "hi_res",
  "hiResModelName": "detectron2_onnx",
  "ocrLanguages": ["eng", "spa", "fra"],
  "chunkingStrategy": "by_title",
  "maxCharacters": 500,
  "coordinates": true,
  "metadata": {
    "source": "company_docs",
    "department": "legal"
  }
}
```

## Best Practices
1. Choose appropriate strategy based on document quality and processing needs
2. Configure OCR languages based on document content
3. Adjust chunking parameters for optimal text segmentation
4. Use appropriate hi-res model for your use case
5. Consider memory usage when processing large folders
6. Monitor API usage and response times
7. Handle potential API errors in your workflow

## Notes
- Process multiple documents in batch
- Supports various file formats
- Memory-efficient processing
- Automatic metadata handling
- Flexible output formats
- Error handling for API responses
- Configurable processing options

{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](broken-reference) to get started.
{% endhint %}
