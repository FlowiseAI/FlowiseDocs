# Supabase

## Condition préalable

1. Enregistrer un compte pour[Supabase](https://supabase.com/)
2. Cliquez sur ** Nouveau projet **

<gigne> <img src = "../../../. GitBook / Assets / Image (8) (2) (1) .png" alt = ""> <Figcaption> </gigcaption> </ Figure>

3. Entrée champs requis

| Nom du champ | Description |
| ------------------------- | ------------------------------------------------- |
| ** Nom ** | Nom du projet à créer. (par exemple Flowise) |
| ** Base de données ** ** Mot de passe ** | Mot de passe à votre base de données Postgres |

<gigne> <img src = "../../../. GitBook / Assets / Image (25) (1) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

4. Cliquez sur ** Créer un nouveau projet ** et attendez que le projet termine la configuration
5. Cliquez sur ** Éditeur SQL **

<gigne> <img src = "../../../. GitBook / Assets / Image (7) (2) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

6. Cliquez sur ** Nouvelle requête **

<gigne> <img src = "../../../. GitBook / Assets / Image (36) (1) .png" alt = ""> <figcaption> </gigcaption> </ figure>

7. Copiez et collez la requête SQL ci-dessous et exécutez-la par`Ctrl + Enter`ou cliquez sur ** Exécuter **. Prenez note du nom de la table et du nom de la fonction.

* ** Nom de la table **:`documents`
* ** Nom de la requête **:`match_documents`

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
as $
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
$;

```

Si certains cas, vous pouvez utiliser[Record Manager](../record-managers.md)pour garder une trace des upserts et éviter les duplications. Étant donné que Record Manager génère un UUID aléatoire pour chaque intégration, vous devrez modifier l'entité de la colonne d'ID en texte:

```sql
-- Enable the pgvector extension to work with embedding vectors
create extension vector;

-- Create a table to store your documents
create table documents (
  id text primary key, -- CHANGE TO TEXT
  content text,
  metadata jsonb,
  embedding vector(1536)
);

-- Create a function to search for documents
create function match_documents (
  query_embedding vector(1536),
  match_count int DEFAULT null,
  filter jsonb DEFAULT '{}'
) returns table (
  id text, -- CHANGE TO TEXT
  content text,
  metadata jsonb,
  similarity float
)
language plpgsql
as $
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
$;

```

<gigne> <img src = "../../../. Gitbook / Assets / Image (19) (1) (1) (1) (2) .png" alt = ""> <figcaption> </gigcaption> </gigne>

## Installation

1. Cliquez sur ** Paramètres du projet **

<gigne> <img src = "../../../. GitBook / Assets / Image (30) (1) .png" alt = ""> <figcaption> </gigcaption> </gigust>

2. Obtenez votre ** URL du projet et la clé API **

<gigne> <img src = "../../../. GitBook / Assets / Image (2) (3) .png" alt = ""> <figcaption> </gigcaption> </gigust>

3. Copiez et collez chaque détail (clé _API, URL, nom de table, nom de requête _) dans ** Supabase ** Node

<gigne> <img src = "../../../. GitBook / Assets / Image (85) .png" alt = "" width = "331"> <Figcaption> </ Figcaption> </ Figure>

4. ** Document ** peut être connecté à n'importe quel nœud sous[**Document Loader**](../document-loaders/)catégorie
5. ** Embeddings ** peut être connecté à n'importe quel nœud sous[**Embeddings** ](../embeddings/)catégorie

## Filtration

Disons que vous avez différents documents renversés, chacun spécifié avec une valeur unique sous la clé de métadonnées`{source}`

<gigne> <img src = "../../../. GitBook / Assets / Untitled.png" alt = ""> <Figcaption> </ Figcaption> </gigne>

Vous pouvez utiliser le filtrage des métadonnées pour interroger les métadonnées spécifiques:

** ui **

<gigne> <img src = "../../../. Gitbook / Assets / Image (9) (1) (1) (1) (1) (2) (1) .png" alt = "" width = "232"> <Figcaption> </ Figcaption> </ Figure>

** API **

```json
"overrideConfig": {
    "supabaseMetadataFilter": {
        "source": "henry"
    }
}
```

## Ressources

* [LangChain JS Supabase](https://js.langchain.com/docs/modules/indexes/vector_stores/integrations/supabase)
* [Supabase Blog Post](https://supabase.com/blog/openai-embeddings-postgres-vector)
* [Metadata Filtering](https://js.langchain.com/docs/integrations/vectorstores/supabase#metadata-filtering)
