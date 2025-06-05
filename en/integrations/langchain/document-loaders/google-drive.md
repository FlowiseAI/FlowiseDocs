# Google Drive

<figure><img src="../../../.gitbook/assets/image (282).png" alt="" width="317"><figcaption></figcaption></figure>

Google Drive is a cloud storage and file synchronization service. This module provides functionality to load and process files from Google Drive, supporting various file formats and Google Workspace documents.

This module provides a sophisticated Google Drive document loader that can:

* Load multiple file types
* Process Google Workspace documents
* Handle folder-based loading
* Support shared drives
* Process files recursively
* Customize file type filtering
* Handle OAuth2 authentication

### Required Parameters

* **Connect Credential**: Google Drive OAuth2 credentials. Refer to [#Google Drive](../tools/google-drive.md)
* **Select Files** or **Folder ID**: Choose specific files or provide a folder ID

### Optional Parameters

* **File Types**: Types of files to load:
  * Google Docs
  * Google Sheets
  * Google Slides
  * PDF Files
  * Text Files
  * Word Documents
  * PowerPoint
  * Excel Files
* **Include Subfolders**: Process files in subfolders
* **Include Shared Drives**: Access files from shared drives
* **Max Files**: Maximum number of files to load (default: 50)
* **Text Splitter**: A text splitter to process the extracted content
* **Additional Metadata**: JSON object with additional metadata
* **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

* **Document**: Array of document objects containing metadata and pageContent
* **Text**: Concatenated string from pageContent of documents

## Supported File Types

### Google Workspace

* Google Docs (application/vnd.google-apps.document)
* Google Sheets (application/vnd.google-apps.spreadsheet)
* Google Slides (application/vnd.google-apps.presentation)

### Microsoft Office

* Word (.docx)
* Excel (.xlsx)
* PowerPoint (.pptx)

### Other Formats

* PDF (.pdf)
* Text Files (.txt)

## Features

* OAuth2 authentication
* Multiple file type support
* Folder processing
* Shared drive access
* File type filtering
* Text splitting support
* Metadata customization
* Error handling

## Loading Methods

### File Selection Mode

* Direct file selection
* Multiple file support
* File type filtering
* Metadata preservation

### Folder Mode

* Recursive folder processing
* Subfolder support
* File type filtering
* Batch processing

## Document Structure

Each document contains:

* **pageContent**: Extracted content from the file
* **metadata**:
  * fileName: Original file name
  * fileType: MIME type
  * fileId: Google Drive file ID
  * source: File path/URL
  * Additional custom metadata

## Notes

* Requires OAuth2 authentication
* Handles rate limiting
* Supports large files
* Temporary file management
* Memory-efficient processing
* Error handling for invalid files
* Automatic token refresh
