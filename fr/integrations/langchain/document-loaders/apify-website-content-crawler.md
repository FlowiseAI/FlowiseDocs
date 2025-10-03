---
description: Load data from Apify Website Content Crawler.
---

# Apify Website Content Crawler

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="266"><figcaption><p>Apify Website Content Crawler Node</p></figcaption></figure>

[Apify](https://apify.com/) Website Content Crawler is a powerful web scraping tool that can extract content from websites using various crawling engines. This module provides integration with Apify's Website Content Crawler to load and process web content.

This module provides a sophisticated web crawler that can:

* Crawl multiple websites from specified start URLs
* Use different crawling engines (Chrome, Firefox, Cheerio, JSDOM)
* Control crawling depth and page limits
* Handle JavaScript-rendered content
* Process extracted content with text splitters
* Customize metadata extraction

## Inputs

### Required Parameters

* **Start URLs**: Comma-separated list of URLs where crawling will begin
* **Connect Apify API**: Apify API credentials
* **Crawler Type**: Choice of crawling engine:
  * Headless web browser (Chrome+Playwright)
  * Stealthy web browser (Firefox+Playwright)
  * Raw HTTP client (Cheerio)
  * Raw HTTP client with JavaScript execution (JSDOM)

### Optional Parameters

* **Text Splitter**: A text splitter to process the extracted content
* **Max Crawling Depth**: Maximum depth of page links to follow (default: 1)
* **Max Crawl Pages**: Maximum number of pages to crawl (default: 3)
* **Additional Input**: JSON object with additional crawler configuration
* **Additional Metadata**: JSON object with additional metadata
* **Omit Metadata Keys**: Comma-separated list of metadata keys to omit

## Outputs

* **Document**: Array of document objects containing metadata and pageContent
* **Text**: Concatenated string from pageContent of documents

## Features

* Multiple crawling engine support
* Configurable crawling parameters
* JavaScript rendering support
* Depth and page limit controls
* Metadata customization
* Text splitting capabilities
* Error handling

## Crawler Types

### Headless Chrome (Playwright)

* Best for modern web applications
* Full JavaScript support
* Higher resource usage

### Stealthy Firefox (Playwright)

* Good for sites with bot detection
* Full JavaScript support
* More stealthy operation

### Cheerio

* Fast and lightweight
* No JavaScript support
* Lower resource usage

### JSDOM (Experimental)

* JavaScript execution support
* Lightweight alternative to browsers
* Experimental features

## Notes

* Requires valid Apify API token
* Different crawler types have different capabilities
* Resource usage varies by crawler type
* JavaScript support depends on crawler type
* Rate limiting may apply based on Apify plan
* Additional configuration available through JSON input

## Crawl Entire Website

1. _(Optional)_ Connect [**Text Splitter**](../text-splitters/).
2. Connect Apify API (create a new credential with your [Apify API token](https://my.apify.com/account#/integrations)).
3. Input one or more URLs (separated by commas) where the crawler will start, e.g `https://docs.flowiseai.com/`.
4. Select the crawler type. Refer to [Website Content Crawler documentation for more information](https://apify.com/apify/website-content-crawler/input-schema#crawlerType).
5. _(Optional)_ Specify additional parameters such as maximum crawling depth and the maximum number of pages to crawl.

## Output

Loads website content as a Document.

## Resources

* [Apify-Flowise integration](https://docs.apify.com/platform/integrations/flowise)
* [Website Content Crawler](https://apify.com/apify/website-content-crawler)
