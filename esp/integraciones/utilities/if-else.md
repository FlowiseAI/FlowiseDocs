# If Else

Flowise te permite dividir tu chatflow en diferentes ramas dependiendo de una condición If/Else.

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

### Variables de Entrada

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Como se observa en la imagen anterior, acepta cualquier nodo que tenga una salida `json`. Algunos ejemplos son: Custom Function, LLM Chain Output Prediction, Get/Set Variables.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

Luego puedes asignar un nombre de variable:

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

Esta variable puede ser utilizada en la [If Function](if-else.md#if-function) y [Else Function](if-else.md#else-function) con el prefijo `$`. Por ejemplo:

```
$output
```

### Nombre del If Else

Puedes nombrar el nodo para una mejor visualización de su función.

### If Function

Este es un fragmento de código JS que se ejecuta en el sandbox de Node. Debe:

* Contener la declaración `if`
* Retornar un valor dentro de la declaración `if`

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt="" width="312"><figcaption></figcaption></figure>

Esto proporciona mucha más flexibilidad a los usuarios para realizar comparaciones complejas como regex, comparación de fechas y mucho más.

### Else Function

Similar a la If Function, debe retornar un valor. Esta función solo se ejecutará si la [If Function](if-else.md#if-function) no retorna un valor.

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt="" width="317"><figcaption></figcaption></figure>

### Salida

<figure><img src="../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

Cuando la [If Function](if-else.md#if-function) retorna exitosamente un valor, este se pasará al punto de salida **True** como se muestra arriba. Esto permite a los usuarios pasar el valor al siguiente nodo.

De lo contrario, el valor retornado por la [Else Function](if-else.md#else-function) se pasará al punto de salida **False**.

Los usuarios también pueden consultar la plantilla If Else en el marketplace:

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>
