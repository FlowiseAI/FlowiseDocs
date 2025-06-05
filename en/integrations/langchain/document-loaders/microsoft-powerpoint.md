# Microsoft PowerPoint Document Loader

<figure><img src="../../../.gitbook/assets/image (286).png" alt="" width="234"><figcaption></figcaption></figure>

Microsoft PowerPoint is a presentation program for creating and displaying slide shows. This module provides functionality to load and process PowerPoint files using officeparser.

This module provides a sophisticated PowerPoint document loader that can:
- Load PowerPoint presentations
- Extract text from slides
- Split content into individual slides
- Handle slide numbering
- Process metadata per slide
- Support multiple slide formats
- Handle various slide separators

## Inputs

### Required Parameters
- **PowerPoint File**: The PowerPoint file(s) to process (.ppt, .pptx)

### Optional Parameters
- **Text Splitter**: A text splitter to process the extracted content
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- Text extraction
- Slide separation
- Metadata handling
- Error handling
- Memory-efficient processing
- Heuristic slide detection
- Content filtering

## Slide Detection Methods

### Pattern Recognition
The loader attempts to identify slides using common patterns:
- "Slide X" markers
- "Page X" markers
- "X/Y" page numbers
- Underscore separators
- Dash separators
- Multiple newlines

### Fallback Mechanisms
If pattern recognition fails:
1. Split by double newlines
2. Treat content as single slide

## Document Structure
Each document contains:
- **pageContent**: Extracted text content from the slide
- **metadata**:
  - slideNumber: Sequential slide number
  - documentType: "powerpoint"
  - Additional custom metadata

## Content Processing
- Empty slides are filtered out
- Leading/trailing whitespace removed
- Minimum content length validation
- Reasonable slide count validation

## Metadata Attributes
Default attributes include:
- slideNumber: Slide number (number)
- documentType: Type of document (string)
- Custom metadata from input

## Notes
- Uses officeparser for extraction
- Handles various slide formats
- Intelligent slide detection
- Content validation
- Memory-efficient processing
- Error handling for invalid files
- Flexible output formats
- Robust fallback mechanisms
