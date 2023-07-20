# Web Scrape QnA

Let's say you have a website (could be a store, an ecommerce site, a blog), and you want to scrap all the relative links of that website and have LLM answer any question on your website. In this tutorial, we are going to go through how to achieve that.

### Upsert Flow

This flow is used to upsert all information from a website to a vector database, then have LLM answer user's question by looking up from the vector database.

<figure><img src="../.gitbook/assets/screely-1689640211454.png" alt=""><figcaption></figcaption></figure>

