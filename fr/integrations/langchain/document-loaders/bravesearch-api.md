# BraveSearch API Document Loader

BraveSearch is a privacy-focused search engine that provides a powerful API for web search. This module enables loading and processing search results from BraveSearch into documents.

This module provides a sophisticated search document loader that can:
- Execute web searches using BraveSearch API
- Convert search results into structured documents
- Extract snippets and metadata from results
- Process results with text splitters
- Customize metadata extraction

## Inputs

### Required Parameters
- **Query**: The search query to execute
- **Connect Credential**: BraveSearch API credentials

### Optional Parameters
- **Text Splitter**: A text splitter to process the extracted content
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- Privacy-focused web search
- Structured result processing
- Automatic metadata extraction
- Result content splitting
- Customizable metadata handling
- Error handling for API responses

## Document Structure
Each search result is converted into a document with:
- **pageContent**: The snippet/content from the search result
- **metadata**:
  - title: The title of the webpage
  - link: The URL of the webpage
  - Additional custom metadata as specified

## Notes
- Requires valid BraveSearch API key
- Results include webpage snippets and metadata
- Can be combined with text splitters for content processing
- Supports custom metadata addition and omission
- Handles API rate limits and errors
- Preserves privacy-focused search features 