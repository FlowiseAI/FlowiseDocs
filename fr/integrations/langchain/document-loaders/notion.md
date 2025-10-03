# Notion

Notion is a collaboration platform that combines note-taking, knowledge management, and project management. This module provides three different loaders to process Notion content: Database, Page, and Folder loaders.

## Notion Database Loader

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="260"><figcaption><p>Notion Database Node</p></figcaption></figure>

The Database loader extracts content from Notion databases, treating each row as a separate document.

### Features

* Load database rows as documents
* Extract properties as metadata
* Support property headers
* Handle concurrent loading
* Process content with text splitters
* Customize metadata extraction

### Required Parameters

* **Connect Credential**: Notion API credentials
* **Database Id**: The unique identifier of the Notion database

## Notion Page Loader

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="262"><figcaption><p>Notion Page Node</p></figcaption></figure>

The Page loader extracts content from Notion pages, including all child pages as separate documents.

### Features

* Load page content as documents
* Process child pages recursively
* Extract page properties
* Handle page hierarchy
* Support text splitting
* Customize metadata extraction

### Required Parameters

* **Connect Credential**: Notion API credentials
* **Page Id**: The 32-character hex identifier from the page URL

## Notion Folder Loader

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="259"><figcaption><p>Notion Folder Node</p></figcaption></figure>

The Folder loader processes exported and unzipped Notion content from a local folder.

### Features

* Process exported Notion content
* Handle multiple pages
* Support local file system
* Extract page content
* Maintain document structure
* Support text splitting
* Customize metadata extraction

### Required Parameters

* **Notion Folder**: Path to the exported and unzipped Notion folder

## Common Features

All Notion loaders support:

### Optional Parameters

* **Text Splitter**: A text splitter to process the extracted content
* **Additional Metadata**: JSON object with additional metadata
* **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

### Outputs

* **Document**: Array of document objects containing metadata and pageContent
* **Text**: Concatenated string from pageContent of documents

## Authentication

### API Authentication (Database & Page Loaders)

* Requires Notion Integration Token
* API rate limiting handled automatically
* Support for workspace-level access
* Secure credential management

### Local Access (Folder Loader)

* No authentication required
* Direct file system access
* Process offline content
* Handle exported data

## Document Structure

Each document contains:

* **pageContent**: Extracted text content
* **metadata**:
  * source: Original source (URL or file path)
  * title: Page or database title
  * properties: Notion properties
  * Additional custom metadata

## Notes

* API loaders require Notion integration setup
* Folder loader needs exported content
* Rate limiting handled automatically
* Memory-efficient processing
* Error handling for invalid inputs
* Support for large datasets
* Flexible output formats
* Metadata customization
