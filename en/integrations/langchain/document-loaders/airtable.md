---
description: Load data from Airtable table.
---

# Airtable Document Loader

<figure><img src="../../../.gitbook/assets/image_airtable (1).png" alt="" width="271"><figcaption><p>Airtable Node</p></figcaption></figure>

Airtable is a cloud collaboration service that combines the functionality of a spreadsheet with a database. This module provides comprehensive functionality to load and process data from Airtable tables.

This module provides a sophisticated Airtable document loader that can:
- Load data from specific Airtable bases, tables, and views
- Filter and select specific fields
- Handle pagination and large datasets
- Support custom filtering with formulas
- Process data with text splitters
- Customize metadata extraction

## Inputs

### Required Parameters
- **Base Id**: The Airtable base identifier (e.g., app11RobdGoX0YNsC)
- **Table Id**: The specific table identifier (e.g., tblJdmvbrgizbYICO)
- **Connect Credential**: Airtable API credentials

### Optional Parameters
- **View Id**: Specific view identifier (e.g., viw9UrP77Id0CE4ee)
- **Text Splitter**: A text splitter to process the extracted content
- **Include Only Fields**: Comma-separated list of field names or IDs to include
- **Return All**: Whether to return all results (default: true)
- **Limit**: Number of results to return when Return All is false (default: 100)
- **Filter By Formula**: Airtable formula to filter records
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- API-based data retrieval
- Field selection and filtering
- Pagination support
- Formula-based filtering
- Customizable metadata handling
- Text splitting capabilities
- Error handling for invalid inputs

## Notes
- Requires valid Airtable API credentials
- Base ID and Table ID are mandatory
- Field names containing commas should use field IDs instead
- Filter formulas must follow Airtable formula syntax
- Rate limiting and API quotas apply
- Supports both full and partial data retrieval

## URL Structure Example
For a table URL like:
```
https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYICO/viw9UrP77Id0CE4ee
```
- Base ID: app11RobdGoX0YNsC
- Table ID: tblJdmvbrgizbYICO
- View ID: viw9UrP77Id0CE4ee

