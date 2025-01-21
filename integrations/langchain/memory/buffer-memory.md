# Buffer Memory

Usa la tabla de base de datos `chat_message` de Flowise como mecanismo de almacenamiento para guardar/recuperar conversaciones.

<figure><img src="../../../.gitbook/assets/image (1) (1) (3).png" alt="" width="299"><figcaption></figcaption></figure>

## Entrada

| Parámetro   | Descripción                                                                      | Valor por defecto |
| ----------- | -------------------------------------------------------------------------------- | ----------------- |
| Session Id  | Un ID para recuperar/almacenar mensajes. Si no se especifica, se usará un ID aleatorio. |               |
| Memory Key  | Una clave usada para formatear mensajes en la plantilla de prompt                | chat_history      |
