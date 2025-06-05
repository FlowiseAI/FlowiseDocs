# Playwright Web Scraper

Playwright is a powerful library for browser automation that can control Chromium, Firefox, and WebKit with a single API. This module provides advanced web scraping capabilities using Playwright to extract content from web pages, including dynamic content that requires JavaScript execution.

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
  - Load: Wait for the load event to fire
  - DOM Content Loaded: Wait for the DOMContentLoaded event
  - Network Idle: Wait until no network connections for 500ms
  - Commit: Wait for initial network response and document loading
- **Wait for selector to load** (optional): CSS selector to wait for before scraping
- **Additional Metadata** (optional): JSON object with additional metadata to add to documents
- **Omit Metadata Keys** (optional): Comma-separated list of metadata keys to omit

## Outputs

- **Document**: Array of document objects containing metadata and pageContent
- **Text**: Concatenated string from pageContent of documents

## Features
- Multi-browser engine support (Chromium, Firefox, WebKit)
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

## Resources

* [LangChain JS Playwright](https://js.langchain.com/docs/integrations/document_loaders/web_loaders/web_playwright)
* [Playwright](https://playwright.dev/)
