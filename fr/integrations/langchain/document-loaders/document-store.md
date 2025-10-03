---
description: Load data from pre-configured document stores.
---

# Document Store

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="278"><figcaption><p>Document Store Node</p></figcaption></figure>

The Document Store loader enables you to load data from pre-configured document stores in your database. This loader provides a convenient way to access and utilize previously processed and stored documents in your workflows.

## Features

* Load documents from synchronized stores
* Automatic metadata handling
* Multiple output formats
* Asynchronous store selection
* Database integration
* Chunk-based document retrieval
* JSON metadata support

## How It Works

1. **Store Selection**:
   * Lists all available document stores that are in 'SYNC' status
   * Provides store information including name and description
   * Allows selection from synchronized stores only
2. **Document Retrieval**:
   * Fetches document chunks from the selected store
   * Reconstructs documents with original metadata
   * Maintains document structure and relationships

## Parameters

### Required Parameters

* **Select Store**: Choose from available synchronized document stores
  * Displays store name and description
  * Only shows stores in 'SYNC' status
  * Dynamically updated based on database content

## Outputs

The loader provides two output formats:

### Document Output

Returns an array of document objects, each containing:

* **pageContent**: The actual content of the document chunk
* **metadata**: Original document metadata in JSON format

### Text Output

Returns a concatenated string containing:

* All document chunks' content
* Separated by newlines
* Properly escaped characters

## Database Integration

The loader integrates with your database through:

* TypeORM data source connection
* Document store entity management
* Chunk-based storage and retrieval
* Metadata preservation

## Document Structure

Each loaded document contains:

```typescript
{
  pageContent: string,    // The actual content
  metadata: {            // Parsed JSON metadata
    // Original document metadata
    // Store-specific information
    // Custom metadata fields
  }
}
```

## Usage Examples

### Basic Store Selection

```json
{
  "selectedStore": "store-id-123"
}
```

### Accessing Document Content

```typescript
// Document output format
[
  {
    "pageContent": "Document content here...",
    "metadata": {
      "source": "original-file.pdf",
      "page": 1,
      "category": "reports"
    }
  }
]

// Text output format
"Document content here...\nNext document content here...\n"
```

## Best Practices

1. Ensure stores are synchronized before access
2. Choose appropriate output format for your use case
3. Handle metadata appropriately in your workflow
4. Consider chunk size when processing large documents
5. Monitor database performance with large stores

## Notes

* Only synchronized stores are available for selection
* Metadata is automatically parsed from JSON
* Documents are reconstructed from chunks
* Supports both document and text output formats
* Integrates with TypeORM for database access
* Handles escape characters in text output
* Maintains original document structure

{% hint style="info" %}
This section is a work in progress. We appreciate any help you can provide in completing this section. Please check our [Contribution Guide](broken-reference/) to get started.
{% endhint %}
