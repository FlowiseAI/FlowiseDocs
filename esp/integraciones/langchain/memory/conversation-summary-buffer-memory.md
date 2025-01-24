# Conversation Summary Buffer Memory

Usa la tabla de base de datos `chat_message` de Flowise como mecanismo de almacenamiento para guardar/recuperar conversaciones.

Esta memoria mantiene un buffer de interacciones recientes y compila las antiguas en un resumen, usando ambas en su almacenamiento. En lugar de eliminar las interacciones antiguas basándose únicamente en su número, ahora considera la longitud total de tokens para decidir cuándo eliminarlas.

<figure><img src="../../../.gitbook/assets/image (4) (1) (2).png" alt="" width="297"><figcaption></figcaption></figure>

## Entrada

| Parámetro       | Descripción                                                                      | Valor por defecto |
| --------------- | -------------------------------------------------------------------------------- | ----------------- |
| Chat Model      | LLM usado para realizar la sumarización                                          |                   |
| Max Token Limit | Resumir conversaciones una vez que se alcance el límite de tokens                | 2000              |
| Session Id      | Un ID para recuperar/almacenar mensajes. Si no se especifica, se usará un ID aleatorio. |               |
| Memory Key      | Una clave usada para formatear mensajes en la plantilla de prompt                | chat_history      |
