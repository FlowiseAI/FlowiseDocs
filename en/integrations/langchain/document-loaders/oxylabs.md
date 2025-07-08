---
description: Get data from any website with Oxylabs.
---

# Oxylabs Document Loaders

Oxylabs is a web scraping service that retrieves public web data at scale, with tools designed to navigate regional restrictions.

<figure><img src="../../../.gitbook/assets/oxylabs_document_loader.png" alt="" width="260"><figcaption><p>Oxylabs Docuemnt Loader Node</p></figcaption></figure>


### Features
- Retrieve data from Google, Amazon and any other website
- Set geolocation
- Utilize the browser rendering
- Parse the data
- Specify User Agent types
- Process content with text splitters

### Required Parameters
- **Connect Credential**: Oxylabs API credentials
- **Query**: Search query or URL
- **Source**: One of the available sources:
  - Universal - scrape any website
  - Google Search - scrape Google Search results
  - Amazon Product - scrape Amazon Product information
  - Amazon Search - scrape Amazon Search results

### Optional Parameters
- **Geolocation**: Sets the proxy's geo location to retrieve data. See [documentation](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FiwDdoZGfMbUe5cRL2417%2Fuploads%2FxoQb19qSyodB2D4no0DZ%2FList%20of%20supported%20geo_location%20values_sapi.json?alt=media&token=d2e2df7b-10ba-4399-a547-0c4a99e62293) for more details.
- **Render**: Enables JavaScript rendering when set to true.
- **Parse**: Returns parsed data when set to true, as long as a dedicated parser exists for the submitted URL's page type.
- **User Agent Type**: Device type and browser.

### Outputs
- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents


## Document Structure
Each document contains:
- **pageContent**: Extracted page content
