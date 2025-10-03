# Google Sheets

<figure><img src="../../../.gitbook/assets/image (283).png" alt="" width="234"><figcaption></figcaption></figure>

Google Sheets is a web-based spreadsheet application. This module provides functionality to load and process data from Google Sheets documents, supporting various data formatting options and sheet selection.

This module provides a sophisticated Google Sheets document loader that can:

* Load data from multiple spreadsheets
* Select specific sheets and ranges
* Handle formatted and unformatted values
* Process formulas and calculations
* Customize header handling
* Process content with text splitters
* Handle OAuth2 authentication

## Inputs

### Required Parameters

* **Connect Credential**: Google Sheets OAuth2 credentials. Refer to [#Google Sheets](../tools/google-sheets.md)
* **Select Spreadsheet**: Choose spreadsheet(s) from your Google Drive

### Optional Parameters

* **Sheet Names**: Comma-separated list of sheet names to load
* **Range**: Specific range to load (e.g., A1:E10)
* **Include Headers**: Whether to include first row as headers (default: true)
* **Value Render Option**: How values should be represented:
  * Formatted Value: As shown in the UI
  * Unformatted Value: Raw values
  * Formula: Original formulas
* **Text Splitter**: A text splitter to process the extracted content
* **Additional Metadata**: JSON object with additional metadata
* **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

* **Document**: Array of document objects containing metadata and pageContent
* **Text**: Concatenated string from pageContent of documents

## Features

* OAuth2 authentication
* Multiple spreadsheet support
* Sheet selection
* Range specification
* Header handling
* Value formatting options
* Text splitting support
* Metadata customization

## Value Render Options

### Formatted Value

* Values as displayed in UI
* Includes formatting
* Numbers with decimals/currency
* Dates in specified format

### Unformatted Value

* Raw cell values
* Numbers without formatting
* Dates as serial numbers
* Boolean as true/false

### Formula

* Original formulas
* Cell references
* Functions
* Calculations

## Document Structure

Each document contains:

* **pageContent**: Formatted sheet content
* **metadata**:
  * spreadsheetId: Google Sheets ID
  * spreadsheetName: Document name
  * sheetName: Sheet name
  * range: Selected range
  * headers: Column headers (if included)
  * lastModified: Last modification date
  * url: Web view link
  * Additional custom metadata

## Notes

* Requires OAuth2 authentication
* Handles rate limiting
* Supports large spreadsheets
* Memory-efficient processing
* Error handling for invalid ranges
* Automatic token refresh
* Real-time data access
