---
description: Load data from a GitHub repository.
---

# GitHub Document Loader

<figure><img src="../../../.gitbook/assets/image (79).png" alt="" width="260"><figcaption><p>Github Node</p></figcaption></figure>

GitHub is a platform for version control and collaboration. This module provides functionality to load and process content from GitHub repositories, supporting both public and private repositories.

This module provides a sophisticated GitHub document loader that can:
- Load content from GitHub repositories
- Support private repository access
- Process repositories recursively
- Handle custom GitHub instances
- Control concurrency and retries
- Customize file filtering
- Process content with text splitters

## Inputs

### Required Parameters
- **Repo Link**: The GitHub repository URL (e.g., https://github.com/FlowiseAI/Flowise)
- **Branch**: The branch to load content from (default: main)

### Optional Parameters
- **Connect Credential**: GitHub API credentials (required for private repos)
- **Recursive**: Whether to process subdirectories
- **Max Concurrency**: Maximum number of concurrent file loads
- **Github Base URL**: Custom GitHub base URL for enterprise instances
- **Github Instance API**: Custom GitHub API URL for enterprise instances
- **Ignore Paths**: Array of glob patterns for paths to ignore
- **Max Retries**: Maximum number of retry attempts
- **Text Splitter**: A text splitter to process the extracted content
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- Public/private repo support
- Enterprise instance support
- Recursive directory processing
- Concurrency control
- Retry mechanism
- Path filtering
- Text splitting support
- Metadata customization

## Authentication Methods

### Public Repositories
- No authentication required
- Rate limits apply
- Limited to public content

### Private Repositories
- Requires GitHub access token
- Higher rate limits
- Access to private content
- Enterprise support

## Document Structure
Each document contains:
- **pageContent**: File content
- **metadata**:
  - source: File path in repository
  - branch: Repository branch
  - commit: Commit hash
  - Additional custom metadata

## Notes
- Supports both public and private repos
- Enterprise GitHub instances supported
- Rate limiting handled automatically
- Exponential backoff for retries
- Path filtering with glob patterns
- Memory-efficient processing
- Error handling for invalid repos
