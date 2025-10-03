---
description: Load data from GitBook.
---

# GitBook

<figure><img src="../../../.gitbook/assets/image (74).png" alt="" width="270"><figcaption><p>GitBook Node</p></figcaption></figure>

# GitBook Document Loader

GitBook is a modern documentation platform that helps teams share knowledge. This module provides functionality to load and process content from GitBook documentation sites.

This module provides a sophisticated GitBook document loader that can:
- Load content from specific GitBook pages
- Crawl entire GitBook documentation sites
- Extract structured content
- Process content with text splitters
- Customize metadata extraction
- Handle recursive page loading

## Inputs

### Required Parameters
- **Web Path**: The URL to the GitBook page or root path
  - Single page: e.g., https://docs.gitbook.com/product-tour/navigation
  - Root path: e.g., https://docs.gitbook.com/

### Optional Parameters
- **Should Load All Paths**: Whether to recursively load all pages from the root path
- **Text Splitter**: A text splitter to process the extracted content
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- Single page loading
- Recursive site crawling
- Content extraction
- Text splitting support
- Metadata customization
- Error handling
- Path management

## Loading Modes

### Single Page Mode
- Loads content from a specific page
- Extracts page content and metadata
- Preserves page structure
- Faster for single page access

### All Paths Mode
- Recursively loads all pages from root
- Maintains site hierarchy
- Extracts all available content
- Preserves navigation structure

## Document Structure
Each document contains:
- **pageContent**: Extracted content from the page
- **metadata**:
  - title: Page title
  - url: Original page URL
  - Additional custom metadata

## Notes
- Supports both single page and full site loading
- Handles GitBook's dynamic content
- Preserves document structure
- Supports custom metadata addition
- Error handling for invalid URLs
- Memory-efficient processing
- Flexible output formats
