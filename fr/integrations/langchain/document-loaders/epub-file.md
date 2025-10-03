# EPUB File Loader

EPUB (Electronic Publication) is a free and open e-book standard by the International Digital Publishing Forum (IDPF). This module provides functionality to load and process EPUB files within your workflow.

This module provides a sophisticated EPUB document loader that can:
- Load single or multiple EPUB files
- Support both base64-encoded files and files from storage
- Extract content per chapter or per file
- Process content with text splitters
- Handle metadata extraction
- Manage temporary file processing

## Inputs

### Required Parameters
- **EPUB File**: The EPUB file(s) to process (.epub extension required)
- **Usage**: Choose between:
  - One document per chapter: Split content by chapters
  - One document per file: Process entire file as one document

### Optional Parameters
- **Text Splitter**: A text splitter to process the extracted content
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- Multiple file processing
- Chapter-level splitting
- File-level processing
- Storage integration
- Metadata customization
- Text splitting support
- Temporary file handling
- Error handling

## Processing Modes

### Per Chapter Mode
- Creates separate documents for each chapter
- Maintains chapter structure
- Preserves chapter metadata
- Better for detailed analysis

### Per File Mode
- Processes entire file as one document
- Maintains overall structure
- Simpler document organization
- Better for overview analysis

## Notes
- Supports both local and storage-based files
- Handles base64 encoded content
- Automatically cleans up temporary files
- Preserves document structure
- Supports custom metadata addition
- Error handling for invalid files
- Memory-efficient processing 