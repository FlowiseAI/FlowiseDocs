# Simple Chat Engine

Un simple chat engine funciona como un pipeline completo para mantener un diálogo entre la IA y el usuario, sin recuperación de contexto. Sin embargo, está equipado con [Memory](../../langchain/memory/), lo que le permite recordar conversaciones.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

## Entradas

* Chat Model
* [Memory](../../langchain/memory/)

## Parámetros

| Nombre         | Descripción                                           |
| -------------- | ----------------------------------------------------- |
| System Message | Una instrucción para el LLM sobre cómo responder la consulta |

## Salidas

| Nombre           | Descripción                            |
| ---------------- | -------------------------------------- |
| SimpleChatEngine | Nodo final para devolver la respuesta  |
