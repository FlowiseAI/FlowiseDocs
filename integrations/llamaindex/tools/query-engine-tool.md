# Query Engine Tool

Convierte un Query Engine en una Tool que puede ser utilizada por [Sub-Question Query Engine](../engine/sub-question-query-engine.md) o Agent.

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

## Entradas

* Vector Store Index

## Parámetros

| Nombre          | Descripción                                                |
| ---------------- | --------------------------------------------------- |
| Tool Name        | Nombre de la tool                                    |
| Tool Description | Una descripción para indicar cuándo el LLM debe usar esta tool |

## Salidas

| Nombre          | Descripción                                                                                      |
| --------------- | ------------------------------------------------------------------------------------------------ |
| QueryEngineTool | Punto de conexión para Agent o [Sub-Question Query Engine](../engine/sub-question-query-engine.md) |
