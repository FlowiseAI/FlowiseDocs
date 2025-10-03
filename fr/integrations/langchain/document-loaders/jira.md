# Jira Document Loader

<figure><img src="../../../.gitbook/assets/image (284).png" alt="" width="234"><figcaption></figcaption></figure>

Jira is a popular issue tracking and project management tool. This module provides functionality to load and process issues from Jira projects, supporting various filtering options and metadata customization.

This module provides a sophisticated Jira document loader that can:
- Load issues from Jira projects
- Filter issues by creation date
- Control batch size for requests
- Process content with text splitters
- Customize metadata extraction
- Handle API authentication

## Inputs

### Required Parameters
- **Connect Credential**: Jira API credentials (username and access token)
- **Host**: Jira instance URL (e.g., https://jira.example.com)
- **Project Key**: The key of the Jira project to load issues from

### Optional Parameters
- **Limit Per Request**: Number of issues to fetch per API request (default: 100)
- **Created After**: Filter issues created after a specific date (e.g., 2024-01-01)
- **Text Splitter**: A text splitter to process the extracted content
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- API token authentication
- Project-based issue loading
- Creation date filtering
- Batch size control
- Text splitting support
- Metadata customization
- Flexible output formats

## Authentication
The loader requires:
- Jira username
- API access token
- Host URL of your Jira instance

## Document Structure
Each document contains:
- **pageContent**: Issue content and description
- **metadata**:
  - Issue-specific metadata (customizable)
  - Project information
  - Creation dates
  - Issue status
  - Additional custom metadata

## Metadata Handling
Two ways to customize metadata:
1. **Additional Metadata**: Add new metadata fields
   - Specify as JSON object
   - Merged with existing metadata

2. **Omit Metadata Keys**: Remove unwanted metadata
   - Comma-separated list of keys
   - Use * to remove all default metadata
   - Nested keys supported (e.g., key1, key2, key3.nestedKey1)

## Notes
- Handles API rate limiting
- Efficient batch processing
- Memory-efficient loading
- Error handling for API requests
- Supports both cloud and self-hosted Jira
- Real-time issue data access
