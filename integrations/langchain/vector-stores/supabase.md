# Supabase

## Prerequisitos

1. Registra una cuenta en [Supabase](https://supabase.com/)
2. Haz clic en **New project**

<figure><img src="../../../.gitbook/assets/image (8) (2) (1).png" alt=""><figcaption></figcaption></figure>

3. Ingresa los campos requeridos

| Nombre del Campo          | Descripción                                       |
| ------------------------- | ------------------------------------------------- |
| **Name**                  | nombre del proyecto a crear (ej. Flowise)         |
| **Database** **Password** | contraseña para tu base de datos postgres         |

<figure><img src="../../../.gitbook/assets/image (25) (1) (1).png" alt=""><figcaption></figcaption></figure>

4. Haz clic en **Create new project** y espera a que el proyecto termine de configurarse
5. Haz clic en **SQL Editor**

<figure><img src="../../../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

6. Haz clic en **New query**

<figure><img src="../../../.gitbook/assets/image (36) (1).png" alt=""><figcaption></figcaption></figure>

7. Copia y pega la siguiente consulta SQL y ejecútala con `Ctrl + Enter` o haciendo clic en **RUN**. Toma nota del nombre de la tabla y el nombre de la función.

* **Nombre de la tabla**: `documents`
* **Nombre de la consulta**: `match_documents`

```plsql
-- Habilita la extensión pgvector para trabajar con vectores de embeddings
create extension vector;

-- Crea una tabla para almacenar tus documentos
create table documents (
  id bigserial primary key,
  content text, -- corresponde a Document.pageContent
  metadata jsonb, -- corresponde a Document.metadata
  embedding vector(1536) -- 1536 funciona para embeddings de OpenAI, cambiar si es necesario
);

-- Crea una función para buscar documentos
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
$$;
```

En algunos casos, podrías estar usando [Record Manager](../record-managers.md) para hacer seguimiento de los upserts y prevenir duplicaciones. Como Record Manager genera un UUID aleatorio para cada embedding, tendrás que cambiar la entidad de la columna id a texto:

```sql
-- Habilita la extensión pgvector para trabajar con vectores de embeddings
create extension vector;

-- Crea una tabla para almacenar tus documentos
create table documents (
  id text primary key, -- CAMBIAR A TEXT
  content text,
  metadata jsonb,
  embedding vector(1536)
);

-- Crea una función para buscar documentos
create function match_documents (
  query_embedding vector(1536),
  match_count int DEFAULT null,
  filter jsonb DEFAULT '{}'
) returns table (
  id text, -- CAMBIAR A TEXT
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
$$;
```

<figure><img src="../../../.gitbook/assets/image (19) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Configuración

1. Haz clic en **Project Settings**

<figure><img src="../../../.gitbook/assets/image (30) (1).png" alt=""><figcaption></figcaption></figure>

2. Obtén tu **Project URL & API Key**

<figure><img src="../../../.gitbook/assets/image (2) (3).png" alt=""><figcaption></figcaption></figure>

3. Copia y pega cada detalle (_API Key, URL, Table Name, Query Name_) en el nodo **Supabase**

<figure><img src="../../../.gitbook/assets/image (85).png" alt="" width="331"><figcaption></figcaption></figure>

4. **Document** puede conectarse con cualquier nodo de la categoría [**Document Loader**](../document-loaders/)
5. **Embeddings** puede conectarse con cualquier nodo de la categoría [**Embeddings**](../embeddings/)

## Filtrado

Supongamos que tienes diferentes documentos insertados, cada uno especificado con un valor único bajo la clave de metadata `{source}`

<figure><img src="../../../.gitbook/assets/Untitled.png" alt=""><figcaption></figcaption></figure>

Puedes usar filtrado de metadata para consultar metadata específica:

**UI**

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1) (2) (1).png" alt="" width="232"><figcaption></figcaption></figure>

**API**

```json
"overrideConfig": {
    "supabaseMetadataFilter": {
        "source": "henry"
    }
}
```

## Recursos

* [LangChain JS Supabase](https://js.langchain.com/docs/modules/indexes/vector_stores/integrations/supabase)
* [Supabase Blog Post](https://supabase.com/blog/openai-embeddings-postgres-vector)
* [Metadata Filtering](https://js.langchain.com/docs/integrations/vectorstores/supabase#metadata-filtering)
