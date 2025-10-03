---
description: Load and process data from web search results.
---

# SerpApi For Web Search

<figure><img src="../../../.gitbook/assets/image (81).png" alt="" width="319"><figcaption><p>SerpApi For Web Search Node</p></figcaption></figure>

The SerpApi For Web Search loader enables you to fetch and process web search results using the SerpApi service. This loader transforms search results into structured documents that can be easily integrated into your workflow, making it ideal for applications requiring real-time web search data.

## Features
- Real-time web search results
- Text splitting capabilities
- Customizable metadata handling
- Multiple output formats
- API key authentication
- Efficient document processing

## Inputs

### Required Parameters
- **Connect Credential**: SerpApi API key credential
- **Query**: The search query to execute

### Optional Parameters
- **Text Splitter**: A text splitter to process the extracted content
- **Additional Metadata**: JSON object with additional metadata to add to documents
- **Omit Metadata Keys**: Comma-separated list of metadata keys to exclude
  - Format: `key1, key2, key3.nestedKey1`
  - Use * to remove all default metadata except custom metadata

## Outputs

- **Document**: Array of document objects containing:
  - metadata: Search result metadata
  - pageContent: Search result content
- **Text**: Concatenated string of all search results' content

## Document Structure
Each document contains:
- **pageContent**: The main content from the search result
- **metadata**:
  - Default search result metadata
  - Custom metadata (if specified)
  - Filtered metadata (based on omitted keys)

## Metadata Handling
Two ways to customize metadata:
1. **Additional Metadata**
   - Add new metadata fields via JSON
   - Merged with existing metadata
   - Useful for adding custom tracking or categorization

2. **Omit Metadata Keys**
   - Remove unwanted metadata fields
   - Comma-separated list of keys to exclude
   - Support for nested key removal
   - Use * to remove all default metadata

## Usage Tips
- Provide specific search queries for better results
- Use text splitters for large search results
- Customize metadata to match your needs
- Consider rate limits when making multiple queries
- Handle search results appropriately based on size

## Notes
- Requires SerpApi API key
- Respects API rate limits
- Real-time search results
- Memory-efficient processing
- Error handling for API requests
- Supports both document and text output formats

## Example Usage
```typescript
// Example search query
query: "artificial intelligence latest developments"

// Example additional metadata
metadata: {
  "source": "serpapi",
  "category": "tech",
  "timestamp": "2024-03-21"
}

// Example metadata keys to omit
omitMetadataKeys: "snippet, position, link"
```
