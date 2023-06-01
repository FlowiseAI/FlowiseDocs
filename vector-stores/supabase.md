# Supabase

## Prerequisite

1. Register an account for [Supabase](https://supabase.com/)
2.  Click **New project**\


    <figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
3.  Input required fields\
    **Name**, name of the project to be created. (e.g. Flowise)\
    **Database** Password, password to your postgres database. (e.g. click **Generate a password**)\


    <figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>
4. Click **Create new project** and wait for the project to finish setting up
5.  Click **SQL Editor**\


    <figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
6.  Click **New query**\


    <figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>
7.  Copy & Paste [query ](https://js.langchain.com/docs/modules/indexes/vector\_stores/integrations/supabase#create-a-table-and-search-function-in-your-database)and run it by `Ctrl + Enter` or click **RUN**\
    **Table name**: documents\
    **Query name**: match\_documents

    ```plsql
    -- Enable the pgvector extension to work with embedding vectors
    create extension vector;

    -- Create a table to store your documents
    create table documents (
      id bigserial primary key,
      content text, -- corresponds to Document.pageContent
      metadata jsonb, -- corresponds to Document.metadata
      embedding vector(1536) -- 1536 works for OpenAI embeddings, change if needed
    );

    -- Create a function to search for documents
    create function match_documents (
      query_embedding vector(1536),
      match_count int DEFAULT null,
      filter jsonb DEFAULT '{}'
    ) returns table (
      id bigint,
      content text,
      metadata jsonb,
      similarity float
    )
    language plpgsql
    as $$
    #variable_conflict use_column
    begin
      return query
      select
        id,
        content,
        metadata,
        1 - (documents.embedding <=> query_embedding) as similarity
      from documents
      where metadata @> filter
      order by documents.embedding <=> query_embedding
      limit match_count;
    end;
    $$;sql

    ```

    <figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

## Setup

1.  Click **Project Settings**\


    <figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>
2.  Get your **Project URL & API Key**

    <figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
3. Copy & Paste each details (_API Key, URL, Table Name, Query Name_) into **Supabase Upsert Document** node or **Supabase Load Existing** node\
   ![](<../.gitbook/assets/image (21).png>)![](<../.gitbook/assets/image (29).png>)
4. **Document** can be connect with any node under [**Document Loader**](../document-loaders.md) category
5. **Embeddings** can be connect with any node under [**Embeddings** ](../embeddings.md)category

## Resources

* [LangChain JS Supabase](https://js.langchain.com/docs/modules/indexes/vector\_stores/integrations/supabase)
* [Supabase Blog Post](https://supabase.com/blog/openai-embeddings-postgres-vector)
