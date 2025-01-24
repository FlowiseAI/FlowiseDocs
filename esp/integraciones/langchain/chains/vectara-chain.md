# Cadena Vectara QA

Una cadena para realizar tareas de preguntas y respuestas con Vectara.

<figure><img src="../../../.gitbook/assets/screely-1700662138252.png" alt=""><figcaption></figcaption></figure>

## Definiciones

**Una cadena de preguntas y respuestas basada en recuperación**, que se integra con un componente de recuperación Vectara y te permite configurar parámetros de entrada y realizar tareas de preguntas y respuestas.

## Entradas

* [Vectara Store](../vector-stores/vectara.md)

## Parámetros

| Nombre                   | Descripción                                                            |
| ----------------------- | ---------------------------------------------------------------------- |
| Summarizer Prompt Name  | modelo a utilizar en la generación del resumen                         |
| Response Language       | idioma deseado para la respuesta                                       |
| Max Summarized Results  | número de resultados principales a usar en el resumen (por defecto 7)  |

## Salidas

| Nombre         | Descripción                            |
| -------------- | -------------------------------------- |
| VectaraQAChain | Nodo final para devolver la respuesta |
