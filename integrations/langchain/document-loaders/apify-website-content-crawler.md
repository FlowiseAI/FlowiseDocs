---
description: Load data from Apify Website Content Crawler.
---

# Apify Website Content Crawler

[Apify](https://apify.com/) is a web scraping and data extraction platform and provides an app store with more than a thousand ready-made cloud tools called Actors. 

The [Website Content Crawler](https://apify.com/apify/website-content-crawler) Actor can deeply crawl websites, clean their HTML by removing a cookies modal, footer, or navigation, and then transform the HTML into Markdown. 
This Markdown can then be store in a vector database for semantic search or Retrieval-Augmented Generation (RAG).

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="266"><figcaption><p>Apify Website Content Crawler Node</p></figcaption></figure>

## Scrape One URL

1.  _(Optional)_ Connect **[Text Splitter](../text-splitters/)**.
2. Connect Apify API (create a new credential with your [Apify API token](https://my.apify.com/account#/integrations)).
3. Input one or more URLs (separate by comma) where the crawler will start.
4. Select crawler type, refer to [Website Content Crawler documentation for more information](https://apify.com/apify/website-content-crawler/input-schema#crawlerType).
5. _(Optional)_ Specify additional parameters such as maximum crawling depth and maximum pages to crawl.

## Output

Loads Website content as Document

## Resources

* [Apify-Flowise integration](https://docs.apify.com/platform/integrations/flowise)
* [Website Content Crawler](https://apify.com/apify/website-content-crawler)
