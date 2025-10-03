---
description: Load data from real-time search results.
---

# SearchApi For Web Search

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="322"><figcaption><p>SearchApi For Web Search</p></figcaption></figure>

The SearchApi For Web Search loader provides access to real-time search results from multiple search engines using the SearchApi service. This loader enables you to fetch, process, and structure search results as documents that can be used in your workflow.

## Features

* Real-time search results from multiple search engines
* Customizable search parameters
* Text splitting capabilities
* Flexible metadata handling
* Multiple output formats
* API key authentication

## Inputs

### Required Parameters

* **Connect Credential**: SearchApi API key credential
* At least one of:
  * **Query**: Search query string
  * **Custom Parameters**: JSON object with search parameters

### Optional Parameters

* **Query**: The search query to execute (if not using custom parameters)
* **Custom Parameters**: JSON object with additional search parameters
  * Supports all parameters from [SearchApi documentation](https://www.searchapi.io/docs/google)
  * Can override default settings
  * Allows engine-specific configurations
* **Text Splitter**: A text splitter to process the extracted content
* **Additional Metadata**: JSON object with additional metadata to add to documents
* **Omit Metadata Keys**: Comma-separated list of metadata keys to exclude
  * Format: `key1, key2, key3.nestedKey1`
  * Use \* to remove all default metadata

## Outputs

* **Document**: Array of document objects containing:
  * metadata: Search result metadata
  * pageContent: Search result content
* **Text**: Concatenated string of all search results' content

## Document Structure

Each document contains:

* **pageContent**: The main content from the search result
* **metadata**:
  * Default search result metadata
  * Custom metadata (if specified)
  * Filtered metadata (based on omitted keys)

## Metadata Handling

Two ways to customize metadata:

1. **Additional Metadata**
   * Add new metadata fields via JSON
   * Merged with existing metadata
   * Useful for adding custom tracking or categorization
2. **Omit Metadata Keys**
   * Remove unwanted metadata fields
   * Comma-separated list of keys to exclude
   * Support for nested key removal
   * Use \* to remove all default metadata

## Usage Tips

* Provide specific search queries for better results
* Use custom parameters for advanced search configurations
* Consider using text splitters for large search results
* Manage metadata to keep relevant information
* Handle rate limits through appropriate query spacing

## Notes

* Requires SearchApi API key
* Respects API rate limits
* Supports multiple search engines
* Real-time search results
* Memory-efficient processing
* Error handling for API requests
