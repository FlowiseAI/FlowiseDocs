# Conversation Summary Memory

Usa la tabla de base de datos `chat_message` de Flowise como mecanismo de almacenamiento para guardar/recuperar conversaciones.

Este tipo de memoria crea un breve resumen de la conversación a lo largo del tiempo. Esto es útil para acortar información de discusiones largas. Actualiza y guarda un resumen actual mientras la conversación continúa. Esto es especialmente útil en chats más largos, donde guardar cada mensaje anterior ocuparía demasiado espacio.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (2).png" alt="" width="296"><figcaption></figcaption></figure>

## Entrada

| Parámetro   | Descripción                                                                      | Valor por defecto |
| ----------- | -------------------------------------------------------------------------------- | ----------------- |
| Chat Model  | LLM usado para realizar la sumarización                                          |                   |
| Session Id  | Un ID para recuperar/almacenar mensajes. Si no se especifica, se usará un ID aleatorio. |               |
| Memory Key  | Una clave usada para formatear mensajes en la plantilla de prompt                | chat_history      |
