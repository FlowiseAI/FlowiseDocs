# Vectara

## Quickstart Tutorial

{% embed url="https://www.youtube.com/watch?v=rBqpvFcD5XY" %}

## Prerequisite

1. Register an account for [Vectara](https://vectara.com/integrations/flowise)
2. Click **Create Corpus**

<figure><img src="../../../.gitbook/assets/vectara/1.png" alt=""><figcaption></figcaption></figure>

Name the corpus to be created and click **Create Corpus** then wait for the corpus to finish setting up.

## Setup

1. Click on the **"Access Control"** tab in the corpus view

<figure><img src="../../../.gitbook/assets/vectara/2.png" alt=""><figcaption></figcaption></figure>

2. Click on the **"Create API Key"** button, choose a name for the API key and pick the **QueryService & IndexService** option

<figure><img src="../../../.gitbook/assets/vectara/3.png" alt=""><figcaption></figcaption></figure>

3. Click **Create** to create the API key
4. Get your **Corpus ID, API Key, and Customer ID** by clicking the down-arrow under "copy" for your new API key:

<figure><img src="../../../.gitbook/assets/vectara/4.png" alt=""><figcaption></figcaption></figure>

5. Back to Flowise canvas, and create your chatflow. Click **Create New** from the Credentials dropdown ane enter your Vectara credentials.

<figure><img src="../../../.gitbook/assets/vectara/5.png" alt="" width="500"><figcaption></figcaption></figure>

6. Enjoy!

## Vectara Query Parameters

For finer control over the Vectara query parameters, click on "**Additional Parameters**" and then you can update the following parameters from their default:

* Metadata Filter: Vectara supports meta-data filtering. To use [filtering](https://docs.vectara.com/docs/common-use-cases/filtering-by-metadata/filter-overview), ensure that metadata fields you want to filter by are defined in your Vectara corpus.
* "Sentences before" and "Sentences after": these control how many sentences before/after the matching text are returned as results from the Vectara retrieval engine
* Lambda: defines the behavior of [hybrid search](https://docs.vectara.com/docs/learn/hybrid-search) in Vectara
* Top-K: how many results to return from Vectara for the query
* MMR-K: number of results to use for [MMR](https://docs.vectara.com/docs/api-reference/search-apis/reranking#maximal-marginal-relevance-mmr-reranker) (max marginal relvance)

<figure><img src="../../../.gitbook/assets/vectara/6.png" alt="" width="500"><figcaption></figcaption></figure>

## Resources

* [LangChain JS Vectara Blog Post](https://blog.langchain.dev/langchain-vectara-better-together/)
* [5 Reasons to Use Vectara's Langchain Integration Blog Post](https://vectara.com/5-reasons-to-use-vectaras-langchain-integration/)
* [Max Marginal Relevance in Vectara](https://vectara.com/blog/get-diverse-results-and-comprehensive-summaries-with-vectaras-mmr-reranker/)
* [Vectara Boomerang embedding model Blog Post](https://vectara.com/introducing-boomerang-vectaras-new-and-improved-retrieval-model/)
* [Detecting Hallucination with Vectara's HHEM](https://vectara.com/blog/cut-the-bull-detecting-hallucinations-in-large-language-models/)
