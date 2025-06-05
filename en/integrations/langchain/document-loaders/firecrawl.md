---
description: Load data from URL using FireCrawl.
---

# FireCrawl

<figure><img src="../../../.gitbook/assets/up-004.png" alt="" width="347"><figcaption><p>FireCrawl Node</p></figcaption></figure>

# FireCrawl Document Loader

FireCrawl is a powerful web crawling and scraping service that provides advanced capabilities for extracting content from websites. This module enables loading and processing web content through the FireCrawl API.

This module provides a sophisticated web crawler that can:
- Scrape single web pages
- Crawl entire websites
- Extract structured data
- Handle JavaScript-rendered content
- Process content with text splitters
- Customize metadata extraction
- Support multiple operation modes

## Inputs

### Required Parameters
- **URL**: The webpage or website URL to process
- **Connect Credential**: FireCrawl API credentials
- **Mode**: Choose between:
  - Scrape: Single page extraction
  - Crawl: Multi-page website crawling
  - Extract: Structured data extraction

### Optional Parameters
- **Text Splitter**: A text splitter to process the extracted content
- **Scrape Options**:
  - Include Tags: HTML tags to include
  - Exclude Tags: HTML tags to exclude
  - Mobile: Use mobile user agent
  - Skip TLS Verification: Bypass SSL checks
  - Timeout: Request timeout
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- Multiple operation modes
- Advanced scraping options
- Structured data extraction
- JavaScript rendering
- Mobile device emulation
- Custom timeout settings
- Error handling

## Operation Modes

### Scrape Mode
- Single page processing
- Main content extraction
- Format selection
- Custom tag filtering

### Crawl Mode
- Multi-page crawling
- Subdomain handling
- Sitemap processing
- Link extraction

### Extract Mode
- Structured data extraction
- Schema-based parsing
- LLM-powered extraction
- Custom extraction prompts

## Document Structure
Each document contains:
- **pageContent**: Extracted content in markdown format
- **metadata**:
  - title: Page title
  - description: Meta description
  - language: Content language
  - sourceURL: Original URL
  - Additional custom metadata

## Notes
- Requires valid FireCrawl API key
- Supports multiple content formats
- Handles rate limiting
- Job status monitoring
- Error handling and retries
- Customizable request options
- Memory-efficient processing
