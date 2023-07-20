# Web Scrape QnA

Let's say you have a website (could be a store, an ecommerce site, a blog), and you want to scrap all the relative links of that website and have LLM answer any question on your website. In this tutorial, we are going to go through how to achieve that.

### Upsert Flow

This flow is used to upsert all information from a website to a vector database, then have LLM answer user's question by looking up from the vector database.

You can find the example flow called - **Conversational Retrieval QA Chain** from the marketplace templates.

Here, we are going to use **Cheerio Web Scraper** node to scrape links from a given URL. Also replacing **RecursiveTextSplitter** to **HtmlToMarkdown** for cleaner data preparation.

<figure><img src="../.gitbook/assets/Untitled (4).png" alt=""><figcaption></figcaption></figure>

If you do not specify anything, by default only the given URL page will be scraped. If you want to crawl the rest of relative links, click **Additional Parameters.**

* **Get Relative Links Method** - how to crawl all relative links, Web Crawl or Sitemap
* **Get Relative Links Limit** - how many links to crawl, set 0 to crawl all

<figure><img src="../.gitbook/assets/image (2).png" alt="" width="563"><figcaption></figcaption></figure>

When you open the chat and start asking question, all links will be scraped and upserted into vector database (Pinecone in this case).

From the console/terminal, you can see all the links that are being scraped:

<figure><img src="../.gitbook/assets/image (5).png" alt="" width="563"><figcaption></figcaption></figure>

Navigate to Pinecone dashboard, you will be able to see new vectors being added under the namespace you have specified in the flow.

<figure><img src="../.gitbook/assets/Untitled (2).png" alt="" width="563"><figcaption></figcaption></figure>

_**Few things to keep in mind**_

* Once the flow is set, documents will only get upserted once when user start asking first question from the UI or API or Embedded Chat.
* Documents will <mark style="color:red;">**not**</mark> get upserted again whenever user ask another question.
* The <mark style="color:red;">**only**</mark> condition where documents get upserted again is when the flow configuration (like different file, different models, different pinecone index, etc) have changed, and you have to save the chatflow again.
* In the other words, we store the flow state, and if a new save is done, we check if `existing state == new state`, if not, do upsert, if yes, ignore.
* However, sometimes you might want to change some settings like metadata but you don't want another upsert to be done again.
* Therefore, it is generally recommended to create another flow to load the existing index from vector store.

### Load Existing Index Flow

This flow is used to load an existing index/collection from vector store, typically after you have upserted the documents to that particular index/collection.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

If you have specified namespace or metadata from the upsert flow, remember to specify here as well, under the **Additional Parameters** in Pinecone node.

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="563"><figcaption></figcaption></figure>

It is recommended to specify a system message for the **Conversational Retrieval QA Chain**. For example, you can specify the name of AI, the language to answer, the response when answer its not found (to prevent hallucination).

{% code overflow="wrap" %}
```
I want you to act as a document that I am having a conversation with. Your name is "AI Assistant". You will provide me with answers from the given info. If the answer is not included, say exactly "Hmm, I am not sure." and stop after that. Refuse to answer any question not about the info. Only answer in English. Never break character.
```
{% endcode %}

<figure><img src="../.gitbook/assets/Untitled (1).png" alt=""><figcaption></figcaption></figure>

That's it! You can start asking question [🤔](https://emojipedia.org/thinking-face/)

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

You can also turn on the Return Source Documents option to return a list of document chunks where the AI's response is coming from.

<figure><img src="../.gitbook/assets/Untitled.png" alt="" width="563"><figcaption></figcaption></figure>

The same logic can be applied to any document use cases, not just limited to web scraping.

If you have any suggestion on how to improve the performance, we'd love your contribution!
