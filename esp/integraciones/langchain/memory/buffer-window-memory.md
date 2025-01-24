# Buffer Window Memory

Usa la tabla de base de datos `chat_message` de Flowise como mecanismo de almacenamiento para guardar/recuperar conversaciones.

La diferencia es que solo recupera las últimas K interacciones. Este enfoque es beneficioso para preservar una ventana deslizante de las interacciones más recientes, asegurando que el buffer mantenga un tamaño manejable.

<figure><img src="../../../.gitbook/assets/image (1) (1) (3) (1).png" alt="" width="298"><figcaption></figcaption></figure>

## Entrada

| Parámetro   | Descripción                                                                      | Valor por defecto |
| ----------- | -------------------------------------------------------------------------------- | ----------------- |
| Size        | Últimos K mensajes a recuperar                                                    | 4                 |
| Session Id  | Un ID para recuperar/almacenar mensajes. Si no se especifica, se usará un ID aleatorio. |               |
| Memory Key  | Una clave usada para formatear mensajes en la plantilla de prompt                | chat_history      |
