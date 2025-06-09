# Cheerio Web Scraper

Cheerio is a fast, flexible, and lean implementation of core jQuery designed specifically for the server. This module provides powerful web scraping capabilities using Cheerio to extract content from web pages.

This module provides a sophisticated web scraper that can:

* Load content from single or multiple web pages
* Crawl relative links from websites
* Extract content using CSS selectors
* Handle XML sitemaps
* Process web content with text splitters

## Inputs

* **URL**: The webpage URL to scrape
* **Text Splitter** (optional): A text splitter to process the extracted content
* **Get Relative Links Method** (optional): Choose between:
  * Web Crawl: Crawl relative links from HTML URL
  * Scrape XML Sitemap: Scrape relative links from XML sitemap URL
* **Get Relative Links Limit** (optional): Limit for number of relative links to process (default: 10, 0 for all links)
* **Selector (CSS)** (optional): CSS selector to target specific content
* **Additional Metadata** (optional): JSON object with additional metadata to add to documents
* **Omit Metadata Keys** (optional): Comma-separated list of metadata keys to omit

## Outputs

* **Document**: Array of document objects containing metadata and pageContent
* **Text**: Concatenated string from pageContent of documents

## Features

* CSS selector-based content extraction
* Web crawling capabilities
* XML sitemap processing
* Configurable link limits
* Error handling for invalid URLs and PDFs
* Metadata customization
* Debug logging support

## Notes

* PDF files are not supported and will be skipped
* Invalid URLs will throw an error
* Setting link limit to 0 will retrieve all available links (may take longer)
* Debug mode provides detailed logging of the scraping process

## Scrape One URL

1. _(Optional)_ Connect [**Text Splitter**](../text-splitters/).
2. Input desired URL to be scraped.

## Crawl & Scrape Multiple URLs

1. Select `Web Crawl` or `Scrape XML Sitemap` in **Get Relative Links Method**.
2. Input `0` in **Get Relative Links Limit** to retrieve all links available from the provided URL.

<figure><img src="../../../.gitbook/assets/image (87).png" alt="" width="563"><figcaption></figcaption></figure>

### Manage Links (Optional)

1. Input desired URL to be crawled.
2. Click **Fetch Links** to retrieve links based on the inputs of the **Get Relative Links Method** and **Get Relative Links Limit** in **Additional Parameters**.
3. In **Crawled Links** section, remove unwanted links by clicking **Red Trash Bin Icon**.
4. Lastly, click **Save**.

<figure><img src="../../../.gitbook/assets/image (88).png" alt="" width="563"><figcaption></figcaption></figure>

## Output

Loads URL content as Document

## Resources

* [LangChain JS Cheerio](https://js.langchain.com/docs/integrations/document_loaders/web_loaders/web_cheerio)
* [Cheerio](https://cheerio.js.org/)
