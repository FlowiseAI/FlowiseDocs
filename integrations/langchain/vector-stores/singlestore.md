# SingleStore

## Configuración

1. Registra una cuenta en [SingleStore](https://www.singlestore.com/)
2. Inicia sesión en el portal. En el panel lateral izquierdo, haz clic en **CLOUD** -> **Create new workspace group.** Luego haz clic en el botón **Create Workspace**.

<figure><img src="../../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

3. Selecciona el proveedor de cloud y la región de datos, luego haz clic en **Next**:

<figure><img src="../../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

4. Revisa y haz clic en **Create Workspace**:

<figure><img src="../../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

5. Ahora deberías ver tu workspace creado:

<figure><img src="../../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

6. Procede a crear una base de datos

<figure><img src="../../../.gitbook/assets/image (65).png" alt="" width="485"><figcaption></figcaption></figure>

7. Deberías poder ver tu base de datos creada y adjunta al workspace:

<figure><img src="../../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

8. Haz clic en Connect desde el menú desplegable del workspace -> Connect Directly:

<figure><img src="../../../.gitbook/assets/image (61).png" alt="" width="418"><figcaption></figcaption></figure>

9. Puedes especificar una nueva contraseña o usar la generada por defecto. Luego haz clic en Continue:

<figure><img src="../../../.gitbook/assets/image (62).png" alt="" width="485"><figcaption></figcaption></figure>

10. En las pestañas, cambia a **Your App**, y selecciona **Node.js** del menú desplegable. Toma nota/guarda el `Username`, `Host`, `Password` ya que los necesitarás en Flowise más adelante.

<figure><img src="../../../.gitbook/assets/image (63).png" alt="" width="563"><figcaption></figcaption></figure>

11. De vuelta al canvas de Flowise, arrastra y suelta los nodos de SingleStore. Haz clic en **Create New** desde el menú desplegable de Credentials:

<figure><img src="../../../.gitbook/assets/image (4) (1) (2) (1).png" alt="" width="271"><figcaption></figcaption></figure>

12. Ingresa el Username y Password de arriba:

<figure><img src="../../../.gitbook/assets/image (64).png" alt="" width="563"><figcaption></figcaption></figure>

13. Luego especifica el Host y Database Name:

<figure><img src="../../../.gitbook/assets/image (5) (1) (2).png" alt="" width="272"><figcaption></figcaption></figure>

14. Ahora puedes comenzar a hacer upsert de datos con SingleStore:

<figure><img src="../../../.gitbook/assets/image (6) (1) (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (7) (1) (2).png" alt=""><figcaption></figcaption></figure>

15. Navega de vuelta al portal de SingleStore, y en tu base de datos, podrás ver todos los datos que se han insertado:

<figure><img src="../../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>
