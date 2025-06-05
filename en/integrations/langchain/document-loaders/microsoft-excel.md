# Microsoft Excel Document Loader

<figure><img src="../../../.gitbook/assets/image (285).png" alt="" width="238"><figcaption></figcaption></figure>

Microsoft Excel is a spreadsheet program that features calculation tools, pivot tables, and a macro programming language. This module provides functionality to load and process Excel files using SheetJS.

This module provides a sophisticated Excel document loader that can:
- Load multiple Excel file formats
- Process multiple worksheets
- Convert rows to structured documents
- Handle various data types
- Preserve cell formatting
- Extract metadata per row
- Support type inference

## Inputs

### Required Parameters
- **Excel File**: The Excel file(s) to process (.xls, .xlsx, .xlsm, .xlsb)

### Optional Parameters
- **Text Splitter**: A text splitter to process the extracted content
- **Additional Metadata**: JSON object with additional metadata
- **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- Multiple format support
- Multi-sheet processing
- Data type preservation
- Metadata extraction
- Type inference
- Error handling
- Memory-efficient processing

## Supported Formats
- Excel Binary (.xls)
- Excel Workbook (.xlsx)
- Excel Macro-Enabled (.xlsm)
- Excel Binary Workbook (.xlsb)

## Data Type Handling

### Supported Types
- Text (string)
- Numbers (number)
- Dates (date)
- Booleans (boolean)
- Formulas (calculated values)
- Empty cells (null)

## Document Structure
Each document contains:
- **pageContent**: Formatted row content as key-value pairs
- **metadata**:
  - worksheet: Sheet name
  - rowNum: Row index
  - Original column values
  - Additional custom metadata

## Row Processing
Each row is converted to a document with:
- Key-value pairs for each cell
- Preserved column headers
- Type information
- Row position

## Metadata Attributes
Default attributes include:
- worksheet: Sheet or Worksheet Name (string)
- rowNum: Row index (number)
- Dynamic attributes based on column headers

## Notes
- Uses SheetJS for parsing
- Preserves data types
- Handles multiple sheets
- Infers column types
- Memory-efficient processing
- Error handling for invalid files
- Flexible output formats
- Column type inference
