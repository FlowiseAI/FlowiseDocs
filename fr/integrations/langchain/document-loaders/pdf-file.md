# PDF Document Loader

PDF (Portable Document Format) is a file format developed by Adobe for presenting documents consistently across software platforms. This module provides functionality to load and process PDF files using pdf.js.

This module provides a sophisticated PDF document loader that can:
- Load single or multiple PDF files
- Split documents by page or file
- Support base64 encoded files
- Handle file storage integration
- Process content with text splitters
- Support legacy PDF versions
- Customize metadata extraction

## Inputs

### Required Parameters
- **PDF File**: The PDF file(s) to process (.pdf extension)
- **Usage**: Choose between:
  - One document per page
  - One document per file

### Optional Parameters
- **Text Splitter**: A text splitter to process the extracted content
- **Use Legacy Build**: Whether to use legacy PDF.js build
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- Multiple file support
- Page-level splitting
- Legacy version support
- Text extraction
- Metadata handling
- Error handling
- Memory-efficient processing

## Processing Modes

### Per Page Mode
- Each page becomes a document
- Preserves page numbers
- Individual page metadata
- Granular content access

### Per File Mode
- Entire PDF as one document
- Combined content
- Single metadata set
- Memory efficient

## Document Structure
Each document contains:
- **pageContent**: Extracted text content
- **metadata**:
  - source: Original file path
  - pdf: PDF-specific metadata
  - page: Page number (in per-page mode)
  - Additional custom metadata

## File Handling

### Local Files
- Direct file loading
- Base64 encoded content
- Multiple file support

### Storage Integration
- File storage system support
- Organization-based storage
- Chatflow-based storage

## Notes
- Uses pdf.js for extraction
- Legacy version support
- Memory-efficient processing
- Error handling for invalid files
- Support for large PDFs
- Flexible output formats
- Metadata customization
- Text encoding handling
