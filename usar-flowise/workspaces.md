# Espacios de Trabajo

{% hint style="info" %}
Los Espacios de Trabajo solo están disponibles para Empresas por ahora. Próximamente en el plan Cloud Pro
{% endhint %}

Al iniciar sesión por primera vez, se generará automáticamente un espacio de trabajo predeterminado para ti. Los espacios de trabajo sirven para dividir recursos entre varios equipos o unidades de negocio. Dentro de cada espacio de trabajo, se utiliza el Control de Acceso Basado en Roles (RBAC) para gestionar permisos y accesos, asegurando que los usuarios tengan acceso solo a los recursos y configuraciones requeridos para su rol.

<figure><img src="../.gitbook/assets/Untitled-2024-10-19-0050.png" alt=""><figcaption></figcaption></figure>

## Configuración de Cuenta de Administrador

<details>

<summary>Para empresas auto-alojadas, se deben establecer las siguientes variables de entorno</summary>

```
JWT_AUTH_TOKEN_SECRET
JWT_REFRESH_TOKEN_SECRET
JWT_ISSUER
JWT_AUDIENCE
JWT_TOKEN_EXPIRY_IN_MINUTES
JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES
PASSWORD_RESET_TOKEN_EXPIRY_IN_MINS
PASSWORD_SALT_HASH_ROUNDS
TOKEN_HASH_SECRET
```

</details>

Por defecto, una nueva instalación de Flowise requerirá una configuración de administrador, similar a cómo tienes que configurar un usuario root para tu base de datos inicialmente.

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt="" width="478"><figcaption></figcaption></figure>

Después de la configuración, el usuario será llevado al panel de control de Flowise. En la barra lateral izquierda, verás la sección de Gestión de Usuarios y Espacios de Trabajo. Un espacio de trabajo predeterminado fue creado automáticamente.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Creando un Espacio de Trabajo

Para crear un nuevo Espacio de Trabajo, haz clic en Agregar Nuevo:

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Te verás agregado como Administrador de la Organización en el espacio de trabajo que creaste.

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

Para invitar nuevos usuarios al espacio de trabajo, primero necesitas crear un Rol.

## Creando un Rol

Navega a Roles en la barra lateral izquierda y haz clic en Agregar Rol:

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

El usuario puede especificar un control granular de permisos para cada recurso. Las únicas excepciones son los recursos en **Gestión de Usuarios y Espacios de Trabajo** (Roles, Usuarios, Espacios de Trabajo, Actividad de Inicio de Sesión). Estos solo están disponibles para el Administrador de la Cuenta por ahora.

Aquí, creamos un rol de editor que tiene acceso a todo. Y otro rol con permisos de solo lectura.

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

## Invitar Usuario

<details>

<summary>Para empresas auto-alojadas, se deben establecer las siguientes variables de entorno</summary>

```
INVITE_TOKEN_EXPIRY_IN_HOURS
SMTP_HOST
SMTP_PORT
SMTP_USER
SMTP_PASSWORD
```

</details>

Navega a Usuarios en la barra lateral izquierda, te verás a ti mismo como administrador de la cuenta. Esto se indica mediante el icono de persona con una estrella:

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

Haz clic en Invitar Usuario e ingresa el correo electrónico a invitar, el espacio de trabajo a asignar y el rol también.

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

Haz clic en Enviar Invitación. El correo electrónico invitado recibirá una invitación:

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

Al hacer clic en el enlace de invitación, el usuario invitado será llevado a una página de Registro.

<figure><img src="../.gitbook/assets/image (10) (1).png" alt="" width="463"><figcaption></figcaption></figure>

Después de registrarse e iniciar sesión como usuario invitado, estarás en el espacio de trabajo asignado, y no habrá sección de Gestión de Usuarios y Espacios de Trabajo:

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

Si eres invitado a múltiples espacios de trabajo, puedes cambiar a diferentes espacios de trabajo desde el botón desplegable en la esquina superior derecha. Aquí estamos asignados al Espacio de Trabajo 2 con permiso de **solo lectura**. Puedes notar que el botón Agregar Nuevo para Flujo de Chat ya no es visible. Esto asegura que el usuario solo pueda ver, no crear, actualizar ni eliminar. Las mismas reglas de RBAC se aplican también para la API.

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

Ahora, de vuelta al Administrador de la Cuenta, podrás ver los usuarios invitados, su estado, roles y espacio de trabajo activo:

<figure><img src="../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

El administrador de la cuenta también puede modificar la configuración de otros usuarios:

<figure><img src="../.gitbook/assets/image (15) (1).png" alt=""><figcaption></figcaption></figure>

## Actividad de Inicio de Sesión

El administrador podrá ver cada inicio y cierre de sesión de todos los usuarios:

<figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

## Creando elementos en el Espacio de Trabajo

Todos los elementos creados en un espacio de trabajo están aislados de otro espacio de trabajo. Los espacios de trabajo son una forma de agrupar lógicamente usuarios y recursos dentro de una organización, asegurando límites de confianza separados para la gestión y el control de acceso a recursos. Se recomienda crear espacios de trabajo distintos para cada equipo.

Aquí, creamos un Flujo de Chat llamado **Chatflow1** en el **Espacio de Trabajo 1**:

<figure><img src="../.gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>

Cuando cambiamos al **Espacio de Trabajo 2**, **Chatflow1** no será visible. Esto se aplica a todos los recursos como Flujos de Agentes, Herramientas, Asistentes, etc.

<figure><img src="../.gitbook/assets/image (17) (1).png" alt=""><figcaption></figcaption></figure>

El diagrama a continuación ilustra la relación entre organizaciones, espacios de trabajo y los diversos recursos asociados y contenidos dentro de un espacio de trabajo.

<figure><img src="../.gitbook/assets/Untitled-2024-10-19-0050.png" alt=""><figcaption></figcaption></figure>

## Compartiendo Credenciales

Puedes compartir credenciales con otros espacios de trabajo. Esto permite a los usuarios reutilizar el mismo conjunto de credenciales en diferentes espacios de trabajo.

Después de crear una credencial, el Administrador de la Cuenta o el usuario con permiso de Compartir Credencial del RBAC podrá hacer clic en Compartir:

<figure><img src="../.gitbook/assets/image (18) (1).png" alt=""><figcaption></figcaption></figure>

El usuario puede seleccionar los espacios de trabajo con los que compartir la credencial:

<figure><img src="../.gitbook/assets/image (19) (1).png" alt=""><figcaption></figcaption></figure>

Ahora, cambia al espacio de trabajo donde se compartió la credencial, verás la Credencial Compartida. El usuario no puede editar credenciales compartidas.

<figure><img src="../.gitbook/assets/image (20) (1).png" alt=""><figcaption></figcaption></figure>

## Eliminando un Espacio de Trabajo

Actualmente solo el Administrador de la Cuenta puede eliminar espacios de trabajo. Por defecto, no puedes eliminar un espacio de trabajo si aún hay usuarios dentro de ese espacio de trabajo.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

Necesitarás desvincular primero a todos los usuarios invitados. Esto permite flexibilidad en caso de que solo quieras eliminar ciertos usuarios de un espacio de trabajo. Ten en cuenta que el Propietario de la Organización que creó el espacio de trabajo no puede ser desvinculado de un espacio de trabajo.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

Después de desvincular a los usuarios invitados, y el único usuario que queda dentro del espacio de trabajo es el Propietario de la Organización, el botón de eliminar ahora se puede hacer clic:

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Eliminar un espacio de trabajo es una acción irreversible y eliminará en cascada todos los elementos dentro de ese espacio de trabajo. Verás un cuadro de advertencia:

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Después de eliminar un espacio de trabajo, el usuario volverá al espacio de trabajo Predeterminado. El espacio de trabajo Predeterminado que se creó automáticamente al inicio no se puede eliminar.
