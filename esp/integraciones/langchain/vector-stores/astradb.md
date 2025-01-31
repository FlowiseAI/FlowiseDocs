# AstraDB

## Configuración

1. Registra una cuenta en [AstraDB](https://astra.datastax.com/)
2. Inicia sesión en el portal. Crea una Base de Datos

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

3. Elige Serverless (Vector), completa el nombre de la Base de Datos, Proveedor y Región

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

4. Después de que la base de datos haya sido configurada, obtén el API Endpoint y genera un Application Token

<figure><img src="../../../.gitbook/assets/Picture7.png" alt=""><figcaption></figcaption></figure>

5. Crea una nueva collection, selecciona la dimensión deseada y la métrica de similitud:

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

6. De vuelta al canvas de Flowise, arrastra y suelta el nodo Astra. Haz clic en **Create New** en el menú desplegable de Credentials:

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (2).png" alt="" width="235"><figcaption></figcaption></figure>

7. Especifica el API Endpoint y el Application Token:

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

8. Ahora puedes hacer upsert de datos a AstraDB

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

9. Navega de vuelta al portal de Astra, y en tu collection, podrás ver todos los datos que se han insertado:

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

10. ¡Comienza a realizar consultas!

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
