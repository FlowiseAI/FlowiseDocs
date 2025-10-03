# Folder with Files Loader

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="262"><figcaption><p>Folder with Files Node</p></figcaption></figure>

The Folder Loader provides functionality to load and process multiple files from a directory. This module supports a wide range of file formats and can recursively process subdirectories.

This module provides a sophisticated folder loader that can:
- Load multiple file types simultaneously
- Process directories recursively
- Handle various document formats
- Support PDF-specific processing
- Process structured data files
- Customize metadata extraction
- Support text splitting

## Inputs

### Required Parameters
- **Folder Path**: Path to the directory containing files
- **Recursive**: Whether to process subdirectories

### Optional Parameters
- **Text Splitter**: A text splitter to process the extracted content
- **PDF Usage**: Choose between:
  - One document per page
  - One document per file
- **JSONL Pointer Extraction**: Pointer name for JSONL files
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Supported File Types

### Documents
- PDF (.pdf)
- Word (.doc, .docx)
- Excel (.xls, .xlsx, .xlsm, .xlsb)
- PowerPoint (.ppt, .pptx)
- Text (.txt)
- Markdown (.md, .markdown)
- HTML (.html)
- XML (.xml)

### Data Files
- JSON (.json)
- JSONL (.jsonl)
- CSV (.csv)

### Programming Languages
- Python (.py, .python)
- JavaScript (.js)
- TypeScript (.ts)
- Java (.java)
- C/C++ (.c, .cpp, .h)
- C# (.cs)
- Ruby (.rb, .ruby)
- Go (.go)
- PHP (.php)
- Swift (.swift)
- Rust (.rs)
- Scala (.scala, .sc)
- Kotlin (.kt)
- Solidity (.sol)

### Web Technologies
- CSS (.css)
- SCSS (.scss)
- LESS (.less)
- SQL (.sql)
- Protocol Buffers (.proto)

## Features
- Multi-format support
- Recursive directory processing
- PDF processing options
- Structured data handling
- Text splitting support
- Metadata customization
- Error handling

## Notes
- Automatically detects file types
- Handles large directories
- Preserves file metadata
- Memory-efficient processing
- Supports custom file extensions
- Error handling for invalid files
- Flexible output formats 