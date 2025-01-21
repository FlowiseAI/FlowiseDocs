# Zep Memory

[Zep](https://github.com/getzep/zep) es un almacén de memoria a largo plazo para aplicaciones LLM. Almacena, resume, incrusta, indexa y enriquece los historiales de aplicaciones/chatbots LLM, y los expone a través de APIs simples y de baja latencia.

## Guía para Desplegar Zep en Render

Puedes desplegar fácilmente Zep en servicios en la nube como [Render](https://render.com/), [Flyio](https://fly.io/). Si prefieres probarlo localmente, también puedes iniciar un contenedor docker siguiendo su [guía rápida](https://github.com/getzep/zep#quick-start).

En este ejemplo, vamos a desplegar en Render.

1. Dirígete al [Repositorio de Zep](https://github.com/getzep/zep#quick-start) y haz clic en **Deploy to Render**
2. Esto te llevará a la página Blueprint de Render y simplemente haz clic en **Create New Resources**

<figure><img src="../../../.gitbook/assets/image (21) (1).png" alt=""><figcaption></figcaption></figure>

3. Cuando el despliegue esté completo, deberías ver 3 aplicaciones creadas en tu panel

<figure><img src="../../../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

4. Simplemente haz clic en la primera llamada **zep** y copia la URL desplegada

<figure><img src="../../../.gitbook/assets/image (38) (1).png" alt=""><figcaption></figcaption></figure>

## Guía para Desplegar Zep en Digital Ocean (vía Docker)

1. Clona el Repositorio

```bash
git clone https://github.com/getzep/zep.git
cd zep
nano .env

```

2. Agrega tu OpenAI API Key en .ENV

```bash
ZEP_OPENAI_API_KEY=

```

```bash
docker compose up -d --build
```

3. Permite el acceso del firewall al puerto 8000

```bash
sudo ufw allow from any to any port 8000 proto tcp
ufw status numbered
```

Si estás usando el firewall separado del panel de Digital Ocean, asegúrate de que el puerto 8000 también esté agregado allí

## Uso en la UI de Flowise

1. De vuelta en la aplicación Flowise, simplemente crea un nuevo lienzo o usa una de las plantillas del marketplace. En este ejemplo, vamos a usar **Simple Conversational Chain**

<figure><img src="../../../.gitbook/assets/Untitled (3) (1).png" alt=""><figcaption></figcaption></figure>

2. Reemplaza **Buffer Memory** con **Zep Memory**. Luego reemplaza la **Base URL** con la URL de Zep que has copiado anteriormente

<figure><img src="../../../.gitbook/assets/Untitled (5).png" alt=""><figcaption></figcaption></figure>

3. Guarda el chatflow y pruébalo para ver si las conversaciones son recordadas.

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

4. Ahora intenta limpiar el historial del chat, deberías ver que ahora no puede recordar las conversaciones anteriores.

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Autenticación de Zep

Zep te permite asegurar tu instancia usando autenticación JWT. Usaremos la utilidad de línea de comandos `zepcli` [aquí](https://github.com/getzep/zepcli/releases).

#### 1. Generar un secreto y el token JWT <a href="#id-1-generate-a-secret-and-the-jwt-token" id="id-1-generate-a-secret-and-the-jwt-token"></a>

Después de descargar ZepCLI:

En Linux o MacOS

```
./zepcli -i
```

En Windows

```
zepcli.exe -i
```

Primero obtendrás tu Token SECRETO:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Luego obtendrás el Token JWT:

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### 2. Configurar variables de entorno de autenticación <a href="#id-2-configure-auth-environment-variables" id="id-2-configure-auth-environment-variables"></a>

Establece las siguientes variables de entorno en tu entorno del servidor Zep:

```
ZEP_AUTH_REQUIRED=true
ZEP_AUTH_SECRET=<el secreto que generaste arriba>
```

#### 3. Configurar Credencial en Flowise <a href="#id-2-configure-auth-environment-variables" id="id-2-configure-auth-environment-variables"></a>

Agrega una nueva credencial para Zep, y coloca el Token JWT en el campo API Key:

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 4. Usar la credencial creada en el nodo Zep <a href="#id-2-configure-auth-environment-variables" id="id-2-configure-auth-environment-variables"></a>

En Connect Credential del nodo Zep, selecciona la credencial que acabas de crear. ¡Y eso es todo!

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
