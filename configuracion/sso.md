# SSO

{% hint style="info" %}
SSO solo está disponible para el Enterprise plan
{% endhint %}

Flowise soporta [OIDC](https://openid.net/) que permite a los usuarios usar _single sign_-on (_SSO_) para acceder a la application. Actualmente solo el [Organization Admin](../using-flowise/workspaces.md#setting-up-admin-account) puede configurar las SSO configurations.

## Microsoft

1. En el Azure portal, busca Microsoft Entra ID:

<figure><img src="../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>

2. Desde la left hand bar, haz click en App Registrations, luego New Registration:

<figure><img src="../.gitbook/assets/image (194).png" alt=""><figcaption></figcaption></figure>

3. Ingresa un app name, y selecciona Single Tenant:

<figure><img src="../.gitbook/assets/image (195).png" alt=""><figcaption></figcaption></figure>

4. Después de que se crea una app, anota el Application (client) ID y Directory (tenant) ID:

<figure><img src="../.gitbook/assets/image (196).png" alt=""><figcaption></figcaption></figure>

5. En la left side bar, haz click en Certificates & secrets -> New client secret -> Add:

<figure><img src="../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

6. Después de que el secret ha sido creado, copia el Value, <mark style="color:red;">no</mark> el Secret ID:

<figure><img src="../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

7. En la left side bar, haz click en Authentication -> Add a platform -> Web:

<figure><img src="../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

8. Completa los redirect URIs. Esto necesitará ser cambiado dependiendo de cómo estés haciendo hosting: `http[s]://[your-flowise-instance.com]/api/v1/azure/callback`:

<figure><img src="../.gitbook/assets/image (218).png" alt="" width="514"><figcaption></figcaption></figure>

9. Deberías poder ver el nuevo Redirect URI creado:

<figure><img src="../.gitbook/assets/image (219).png" alt=""><figcaption></figcaption></figure>

10. De vuelta a la Flowise app, haz login como Organization Admin. Navega a SSO Config desde la left side bar. Completa el Azure Tenant ID y Client ID del Paso 4, y Client Secret del Paso 6. Haz click en Test Configuration para ver si la conexión puede establecerse exitosamente:

<figure><img src="../.gitbook/assets/image (220).png" alt="" width="563"><figcaption></figcaption></figure>

11. Por último, enable y save:

<figure><img src="../.gitbook/assets/image (221).png" alt="" width="563"><figcaption></figcaption></figure>

12. Antes de que los users puedan sign in usando SSO, tienen que ser invited primero. Consulta [Inviting users for SSO sign in](sso.md#inviting-users-for-sso-sign-in) para una guía paso a paso. Los invited users también deben ser parte de los Directory Users en Azure.

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

## Google

Para enable Sign In With Google en tu website, primero necesitas configurar tu Google API client ID. Para hacerlo, completa los siguientes pasos:

1. Abre la página **Credentials** de la [Google APIs console](https://console.developers.google.com/apis).
2. Haz click en **Create credentials** > **OAuth client ID**

<figure><img src="../.gitbook/assets/image (224).png" alt="" width="563"><figcaption></figcaption></figure>

3. Selecciona **Web Application**:

<figure><img src="../.gitbook/assets/image (225).png" alt="" width="504"><figcaption></figcaption></figure>

4. Completa los redirect URIs. Esto necesitará ser cambiado dependiendo de cómo estés haciendo hosting: `http[s]://[your-flowise-instance.com]/api/v1/google/callback`:

<figure><img src="../.gitbook/assets/image (226).png" alt="" width="563"><figcaption></figcaption></figure>

5. Después de crear, obtén el client ID y secret:

<figure><img src="../.gitbook/assets/image (227).png" alt=""><figcaption></figcaption></figure>

6. De vuelta a la Flowise app, agrega el Client ID y secret. Test la conexión y Save.

<figure><img src="../.gitbook/assets/image (228).png" alt="" width="563"><figcaption></figcaption></figure>

## Auth0

1. Registra una cuenta en [Auth0](https://auth0.com/), luego crea una nueva Application

<figure><img src="../.gitbook/assets/image (229).png" alt=""><figcaption></figcaption></figure>

2. Selecciona **Regular Web Application**:

<figure><img src="../.gitbook/assets/image (230).png" alt=""><figcaption></figcaption></figure>

3. Configura los fields como Name, Description. Toma nota del **Domain**, **Client ID**, y **Client Secret**.

<figure><img src="../.gitbook/assets/image (231).png" alt=""><figcaption></figcaption></figure>

4. Completa los Application URIs. Esto necesitará ser cambiado dependiendo de cómo estés haciendo hosting: `http[s]://[your-flowise-instance.com]/api/v1/auth0/callback`:

<figure><img src="../.gitbook/assets/image (232).png" alt=""><figcaption></figcaption></figure>

5. En el tab de API's, asegúrate de que Auth0 Management API está enabled con los siguientes permissions
   * read:users
   * read:client_grants

<figure><img src="../.gitbook/assets/image (233).png" alt=""><figcaption></figcaption></figure>

6. De vuelta a la Flowise App, completa el Domain, Client ID y Secret. Test y Save la configuration.

<figure><img src="../.gitbook/assets/image (234).png" alt="" width="563"><figcaption></figcaption></figure>

## Inviting users for SSO sign in

Para que un nuevo user pueda login usando SSO, tenemos que invite new users a la Flowise application. Esto es esencial para mantener un registro del role/workspace del invited user. Consulta la sección [Invite Users](../using-flowise/workspaces.md#invite-user) para la configuración de environment variables.

El Organization Admin puede elegir el login type para el invited user:

<figure><img src="../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>

* SSO: el invited user solo puede login usando SSO
* Email/Password: el invited user solo puede login vía email/password

El invited user recibirá un invitation link para login:

<figure><img src="../.gitbook/assets/image (222).png" alt="" width="449"><figcaption></figcaption></figure>

Hacer click en el button llevará al invited user directamente a la pantalla de Flowise SSO login:

<figure><img src="../.gitbook/assets/image (210).png" alt="" width="400"><figcaption></figcaption></figure>

O navega a la Flowise app y Sign in with SSO:

<figure><img src="../.gitbook/assets/image (211).png" alt="" width="437"><figcaption></figcaption></figure>
