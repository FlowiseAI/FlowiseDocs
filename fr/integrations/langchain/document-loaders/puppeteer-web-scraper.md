# Puppeteer Web Scraper

Puppeteer is a Node.js library that provides a high-level API to control Chrome/Chromium over the DevTools Protocol. This module provides advanced web scraping capabilities using Puppeteer to extract content from web pages, including dynamic content that requires JavaScript execution.

This module provides a sophisticated web scraper that can:
- Load content from single or multiple web pages
- Handle JavaScript-rendered content
- Support various page load strategies
- Wait for specific elements to load
- Crawl relative links from websites
- Process XML sitemaps

## Inputs

- **URL**: The webpage URL to scrape
- **Text Splitter** (optional): A text splitter to process the extracted content
- **Get Relative Links Method** (optional): Choose between:
  - Web Crawl: Crawl relative links from HTML URL
  - Scrape XML Sitemap: Scrape relative links from XML sitemap URL
- **Get Relative Links Limit** (optional): Limit for number of relative links to process (default: 10, 0 for all links)
- **Wait Until** (optional): Page load strategy:
  - Load: When initial HTML document's DOM is loaded
  - DOM Content Loaded: When complete HTML document's DOM is loaded
  - Network Idle 0: No network connections for 500ms
  - Network Idle 2: No more than 2 network connections for 500ms
- **Wait for selector to load** (optional): CSS selector to wait for before scraping
- **Additional Metadata** (optional): JSON object with additional metadata to add to documents
- **Omit Metadata Keys** (optional): Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- JavaScript execution support
- Configurable page load strategies
- Element wait capabilities
- Web crawling functionality
- XML sitemap processing
- Headless browser operation
- Sandbox configuration
- Error handling for invalid URLs
- Metadata customization

## Notes
- Runs in headless mode by default
- Uses no-sandbox mode for compatibility
- Invalid URLs will throw an error
- Setting link limit to 0 will retrieve all available links (may take longer)
- Supports waiting for specific DOM elements before extraction

## Scrape One URL

1.  _(Optional)_ Connect **[Text Splitter](../text-splitters/)**.
2. Input desired URL to be scraped.

## Crawl & Scrape Multiple URLs
Visit **[Web Crawl](../../use-cases/web-crawl.md)** guide to allow scraping of multiple pages.

## Output

Loads URL content as Document

## Resources

* [LangChain JS Puppeteer](https://js.langchain.com/docs/integrations/document_loaders/web_loaders/web_puppeteer)
* [Puppeteer](https://pptr.dev/)
