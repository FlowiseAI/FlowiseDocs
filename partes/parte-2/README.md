# Parte 2: Chains Avanzadas

En esta segunda parte del curso, veremos en profundidad las Chains, uno de los recursos más fundamentales de Flowise. Veremos técnicas como el encadenamiento de Chains, input de imágenes, moderación y cómo conseguir una salida de datos estructurada mediante Output Parsers.

## Contenidos

- [Tipos de Chains](../../integraciones/langchain/chains/README.md)
- [Cache](#cache)
- [Encadenamiento de Chains](#encadenamiento-de-chains)
- [Input de imágenes](#input-de-imágenes)
- [Prompts](#prompts)
- [Output Parsers](#output-parsers)
- [Moderación](#moderación)

## Cache

El cache es como una "memoria temporal de respuestas". Imagina que es como una libreta donde Flowise apunta las preguntas y respuestas que ya se han hecho, para no tener que preguntarle de nuevo a la IA lo mismo.

### ¿Por qué es útil?

- Ahorra dinero: Si alguien hace una pregunta que ya se hizo antes, Flowise usa la respuesta guardada en vez de hacer una nueva consulta al modelo de IA 
- Es más rápido: En vez de esperar la respuesta de la IA, Flowise puede dar inmediatamente la respuesta que tenía guardada

### Un ejemplo práctico:

Usuario 1 pregunta: "¿Qué es Flowise?"

Flowise consulta a la IA y guarda la respuesta en el cache


Usuario 2 pregunta: "¿Qué es Flowise?"

Flowise lee la respuesta del cache y no necesita consultar a la IA de nuevo

Es como tener un bloc de notas rápido donde consultar antes de hacer una llamada (que cuesta tiempo y dinero) a la IA.

![Diagrama Simplificado del Funcionamiento del Cache](/../../.gitbook/assets/partes/parte2/Cache1.png)

## Encadenamiento de Chains

El encadenamiento de Chains consiste en usar el output de una Chain en el input de otra Chain.

![Ejemplo de encadenamiento de Chains](/../../.gitbook/assets/partes/parte2/ChainsEncadenadas.png)

Se realiza principalmente con LLM Chains, toda chain que quiera ser usada como input dentro de otra chain se debe de poner en modo "Output Prediction".

![OutputPrediction](/../../.gitbook/assets/partes/parte2/OutputPrediction.png)

Para conectar dos chains debes de usar el output de una chain (en modo Output Prediction) como input en el prompt de la otra chain.

![Dos Chains concectadas](/../../.gitbook/assets/partes/parte2/Chainsconectadas.png)

Finalmente, debes de asegurarte dentro de la prompt que seleccionas el output de la primera chain, dentro de "Format Prompt Values".

![Output Seleccionado](/../../.gitbook/assets/partes/parte2/Input.png)

## Input de imágenes

Para permitir que un modelo de IA reciba imágenes como input, hay que activar la opción "Allow Image Uploads".

![Allow Image Uploads](/../../.gitbook/assets/partes/parte2/ImagesUpload.png)

## Prompts 

Los Prompts son como las recetas que le das a la inteligencia artificial (IA) para que sepa qué hacer. Son las instrucciones que le permiten entender el contexto y generar respuestas relevantes.

Piensa en la IA como un chef muy talentoso que no sabe qué cocinar. Tú, como usuario, tienes una idea de lo que quieres (por ejemplo, "Quiero un resumen de este artículo"). El Prompt es la receta que le das al chef. 

Ejemplo:

Si quieres que la IA te escriba un poema, un buen Prompt podría ser: "Escribe un poema corto sobre la naturaleza, usando rimas consonantes y un tono melancólico."

![Nodos de Prompts](/../../.gitbook/assets/partes/parte2/NodosPrompts.png)

## Output Parsers

Los Output Parsers son herramientas que transforman la respuesta de la IA a un formato más útil y estructurado. Los modelos de lenguaje a veces generan texto libre, que puede ser difícil de procesar directamente por otras aplicaciones o sistemas. Los Output Parsers se encargan de analizar y formatear esa salida.

![Ejemplo de Output Parser](/../../.gitbook/assets/partes/parte2/OutputParser1.png)

## Moderación

La Moderación es como un guardián que se encarga de que la conversación sea segura y apropiada. Revisa tanto lo que entra (input) como lo que sale (output) del modelo de IA, para asegurarse de que no haya contenido ofensivo, dañino o inapropiado.


## Links Relevantes

- [Chains](../../integraciones/langchain/chains/README.md)
- [Cache](../../integraciones/langchain/cache/README.md)
- [Prompts](../../integraciones/langchain/prompts/README.md)
- [Output Parsers](../../integraciones/langchain/output-parsers/README.md)
- [Moderation](../../integraciones/langchain/moderation/README.md)

