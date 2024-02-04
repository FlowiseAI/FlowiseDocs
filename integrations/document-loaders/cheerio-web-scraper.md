# Cheerio Web Scrapper

Cheerio is lightweight and doesn't require a full browser environment like some other scraping tools. Keep in mind that when scraping websites, **you should always review and comply with the website's terms of service and policies to ensure ethical and legal use of the data**.

## Scrape one page

1.  _(Optional)_ Connect **[Text Splitter](../text-splitters/)**.
2. Input desired URL to be scrape.

## Crawl and scrape multiple pages
1. Click **Additional Parameters**.
2. Select `Web Crawl` or `Scrape XML Sitemap` in **Get Relative Links Method**.
3. Input `0` in **Get Relative Links Limit** to retrieve all links available in the provided URL.

## Mange Links
1. Click **Manage Links**.
2.  _(Optional)_ Input dsired URL to be scrape.
1. Click **Fetch Links** will retrieve links based on the inputs of the **Get Relative Links Method** and **Get Relative Links Limit** in **Additional Parameters**.
2. In **Scraped Links** section, remove unwanted links by clicking **Red Trash Bin Icon**.
3. Lastly, click **Save**.

## Output

Loads URL content as Document

## Resources

* [LangChain JS Cheerio](https://js.langchain.com/docs/integrations/document_loaders/web_loaders/web_cheerio)
* [Cheerio](https://cheerio.js.org/)
