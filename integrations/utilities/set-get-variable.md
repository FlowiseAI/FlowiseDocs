# Set/Get Variable

Si estás ejecutando una Custom Function o LLM Chain, es posible que desees reutilizar el resultado en otros nodos sin tener que recalcular/reejecutar lo mismo nuevamente. Puedes guardar el resultado de salida como una variable y reutilizarlo en otros nodos que estén más adelante en el flujo.

<figure><img src="../../.gitbook/assets/savereuse.png" alt=""><figcaption></figcaption></figure>

### Set Variable

Tomando entradas de cualquier nodo que produzca salidas de tipo `string, number, boolean, json, array,` podemos asignarle un nombre de variable.

<figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1) (1).png" alt="" width="270"><figcaption></figcaption></figure>

### Get Variable

Puedes obtener el valor de la variable usando el nombre de la variable en una etapa posterior:

<figure><img src="../../.gitbook/assets/image (12) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>
