# Web Scrape QnA

Let's say you have a website (could be a store, an ecommerce site, a blog), and you want to scrap all the relative links of that website and have LLM answer any question on your website. In this tutorial, we are going to go through how to achieve that.

You can find the example flow called - **WebPage QnA** from the marketplace templates.

### Upsert

We are going to use **Cheerio Web Scraper** node to scrape links from a given URL.

**HtmlToMarkdown Text Splitter** to split the scraped content into smaller pieces.

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

If you do not specify anything, by default only the given URL page will be scraped. If you want to crawl the rest of relative links, click **Additional Parameters.**

* **Get Relative Links Method** - how to crawl all relative links, Web Crawl or Sitemap
* **Get Relative Links Limit** - how many links to crawl, set 0 to crawl all

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

On the top right corner, you will notice a green button:

<figure><img src="../.gitbook/assets/Untitled (2).png" alt=""><figcaption></figcaption></figure>

A dialog will be shown that allow users to upsert data to Pinecone:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Under the hood, following actions will be executed:

1. Scraped all HTML data using Cheerio Web Scraper
2. Convert all scraped data from HTML to Markdown, then split it
3. Splitted data will be looped over, and converted to vector embeddings using OpenAI Embeddings
4. Vector embeddings will be upserted to Pinecone

Navigate to Pinecone dashboard, you will be able to see new vectors being added.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Query

Querying is relatively straight-forward. After you have verified that data is upserted to vector database, you can start asking question in the chat:

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

It is recommended to specify a system message for the **Conversational Retrieval QA Chain**. For example, you can specify the name of AI, the language to answer, the response when answer its not found (to prevent hallucination).

{% code overflow="wrap" %}
```
I want you to act as a document that I am having a conversation with. Your name is "AI Assistant". You will provide me with answers from the given info. If the answer is not included, say exactly "Hmm, I am not sure." and stop after that. Refuse to answer any question not about the info. Only answer in English. Never break character.
```
{% endcode %}

<figure><img src="../.gitbook/assets/Untitled (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

You can also turn on the Return Source Documents option to return a list of document chunks where the AI's response is coming from.

<figure><img src="../.gitbook/assets/Untitled (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

The same logic can be applied to any document use cases, not just limited to web scraping.

If you have any suggestion on how to improve the performance, we'd love your contribution!
