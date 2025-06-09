---
description: Aprende cómo configurar el control de acceso a nivel de aplicación para tus instancias de Flowise
---

# Aplicación

***

## Email y Contraseña

A partir de la versión v3.0.1, se introdujo un nuevo método de autenticación. Flowise utiliza un **sistema de autenticación basado en** [**Passport.js**](https://www.passportjs.org/) **con tokens JWT** almacenados en cookies HTTP-only seguras. Cuando un usuario inicia sesión, el sistema valida su email/contraseña contra la base de datos usando comparación de hash bcrypt, luego genera dos tokens JWT: un token de acceso de corta duración (por defecto 60 minutos) y un token de actualización de larga duración (por defecto 90 días). Estos tokens se almacenan como cookies seguras. Para solicitudes posteriores, el sistema extrae el JWT de las cookies, valida la firma y las claims usando la estrategia JWT de Passport, y verifica que la sesión de usuario aún exista. El sistema también soporta actualización automática de tokens cuando el token de acceso expira, mantiene sesiones usando Redis o almacenamiento en base de datos dependiendo de la configuración.

Para usuarios existentes que han estado usando [#Username & Password (Deprecado)](nivel-app.md#username-y-password-deprecado), necesitas configurar una nueva cuenta de administrador. Para prevenir reclamos de propiedad no autorizados, primero debes autenticarte usando el username y password existentes configurados como `FLOWISE_USERNAME` y `FLOWISE_PASSWORD`.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="387"><figcaption></figcaption></figure>

Las siguientes variables de entorno pueden ser modificadas:

### URL de la Aplicación

* `APP_URL` - La URL de tu aplicación Flowise alojada. Por defecto `http://localhost:3000`

### Configuración de Variables de Entorno JWT

Para configurar los parámetros de autenticación JWT de Flowise, el usuario puede modificar las siguientes variables de entorno:

* `JWT_AUTH_TOKEN_SECRET` - La clave secreta para firmar tokens de acceso
* `JWT_REFRESH_TOKEN_SECRET` - Secreto para tokens de actualización (por defecto usa el secreto del token de autenticación si no se establece)
* `JWT_TOKEN_EXPIRY_IN_MINUTES` - Duración del token de acceso (por defecto: 60 minutos)
* `JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES` - Duración del token de actualización (por defecto: 129,600 minutos o 90 días)
* `JWT_AUDIENCE` - Claim de audiencia para validación de token (por defecto: 'AUDIENCE')
* `JWT_ISSUER` - Claim de emisor para validación de token (por defecto: 'ISSUER')
* `EXPRESS_SESSION_SECRET` - Secreto de encriptación de sesión (por defecto: 'flowise')
* `EXPIRE_AUTH_TOKENS_ON_RESTART` - Establecer a 'true' para invalidar todos los tokens al reiniciar el servidor (útil para desarrollo)

### Configuración de Email SMTP

Configura estas variables para habilitar funcionalidad de email para restablecimiento de contraseñas y notificaciones:

* `SMTP_HOST` - El hostname de tu servidor SMTP (ej: `smtp.gmail.com`, `smtp.host.com`)
* `SMTP_PORT` - El número de puerto para conexión SMTP (valores comunes: `587` para TLS, `465` para SSL, `25` para no encriptado)
* `SMTP_USER` - Username para autenticación SMTP (usualmente tu dirección de email)
* `SMTP_PASSWORD` - Contraseña o contraseña específica de app para autenticación SMTP
* `SMTP_SECURE` - Establecer a `true` para encriptación SSL/TLS, `false` para conexiones no encriptadas
* `ALLOW_UNAUTHORIZED_CERTS` - Establecer a `true` para permitir certificados autofirmados (no recomendado para producción)
* `SENDER_EMAIL` - La dirección de email "from" que aparecerá en emails salientes

### Configuración de Seguridad y Tokens

Estas variables controlan la seguridad de autenticación, expiración de tokens, y hashing de contraseñas:

* `PASSWORD_RESET_TOKEN_EXPIRY_IN_MINS` - Tiempo de expiración para tokens de restablecimiento de contraseña (por defecto: 15 minutos)
* `PASSWORD_SALT_HASH_ROUNDS` - Número de rondas de salt bcrypt para hashing de contraseñas (por defecto: 10, más alto = más seguro pero más lento)
* `TOKEN_HASH_SECRET` - Clave secreta usada para hacer hash de tokens y datos sensibles (usa una cadena fuerte y aleatoria)

### Mejores Prácticas de Seguridad

* Usa valores fuertes y únicos para `TOKEN_HASH_SECRET` y almacénalos de forma segura
* Para producción, usa `SMTP_SECURE=true` y `ALLOW_UNAUTHORIZED_CERTS=false`
* Establece tiempos de expiración de tokens apropiados basados en tus requerimientos de seguridad
* Usa valores más altos de `PASSWORD_SALT_HASH_ROUNDS` (12-15) para mejor seguridad en producción

##

## Username y Password (Deprecado)

La autorización a nivel de aplicación protege tu instancia de Flowise mediante username y password. Esto protege tus apps de ser accesibles por cualquier persona cuando están deployed online.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Cómo Establecer Username y Password

#### NPM

1. Install Flowise

```bash
npm install -g flowise
```

2. Start Flowise con username y password

```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

3. Open [http://localhost:3000](http://localhost:3000)

#### Docker

1. Navigate a la carpeta `docker`

```
cd docker
```

2. Create un archivo `.env` y especifica el `PORT`, `FLOWISE_USERNAME`, y `FLOWISE_PASSWORD`

```sh
PORT=3000
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```

3. Pass `FLOWISE_USERNAME` y `FLOWISE_PASSWORD` al archivo `docker-compose.yml`:

```
environment:
    - PORT=${PORT}
    - FLOWISE_USERNAME=${FLOWISE_USERNAME}
    - FLOWISE_PASSWORD=${FLOWISE_PASSWORD}
```

4. `docker compose up -d`
5. Open [http://localhost:3000](http://localhost:3000)
6. Puedes detener los containers usando `docker compose stop`

#### Git clone

Para habilitar la app level authentication, add `FLOWISE_USERNAME` y `FLOWISE_PASSWORD` al archivo `.env` en `packages/server`:

```
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```
