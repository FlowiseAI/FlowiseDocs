# Parte 1: Introducción

En esta primera parte del curso, aprenderás los fundamentos esenciales para comenzar con Flowise. Cubriremos desde la instalación básica hasta conceptos fundamentales que necesitarás para crear tus primeros chatflows.

## Contenidos
- [Instalación de Flowise](#instalación-de-flowise)
- [Chains](#chains)
- [Chat Models](#chat-models)
- [Memoria](#memoria)
- [Cómo estar al día con Flowise](#cómo-estar-al-día-con-flowise)

## Instalación de Flowise

### Requisitos Previos
- Node.js >= 18.15.0

### Instalación Básica
```bash
npm install -g flowise
```

### Iniciar Flowise
```bash
npx flowise start
```

### Iniciar con usuario y contraseña
```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

Acceder a http://localhost:3000

### Actualizar Flowise
```bash
npm update -g flowise
```

## Docker

### Docker Compose
1. Clonar el proyecto Flowise
2. Ir a la carpeta docker en la raíz del proyecto
3. Copiar el archivo .env.example, pegarlo en la misma ubicación y renombrarlo a .env
4. Ejecutar:
```bash
docker compose up -d
```
5. Acceder a http://localhost:3000
6. Para detener los contenedores: `docker compose stop`

### Docker Image
Construir la imagen localmente:
```bash
docker build --no-cache -t flowise .
```

Ejecutar imagen:
```bash
docker run -d --name flowise -p 3000:3000 flowise
```

Detener imagen:
```bash
docker stop flowise
```

### Actualizar Flowise con Docker
1. Navegar al directorio donde está instalado Flowise:
```bash
cd Flowise/docker
```

2. Detener y eliminar el contenedor actual:
```bash
sudo docker compose stop
sudo docker compose rm
```

3. Descargar la última imagen de Flowise:
```bash
docker pull flowiseai/flowise
```

4. Reiniciar el contenedor con la imagen actualizada:
```bash
docker compose up -d
```

Nota: Este método asegura que tus flujos y datos permanezcan intactos ya que se almacenan en una carpeta de base de datos separada.

## Instalación Local

### Módulos
Flowise tiene 3 módulos diferentes en un único repositorio mono:
- server: Backend Node para servir lógicas API
- ui: Frontend React
- components: Integraciones de nodos de terceros
- api-documentation: Documentación API swagger-ui autogenerada desde express

### Prerequisitos
Instalar PNPM:
```bash
npm i -g pnpm
```

### Configuración
1. Clonar el repositorio:
```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2. Ir a la carpeta del repositorio:
```bash
cd Flowise
```

3. Instalar todas las dependencias de todos los módulos:
```bash
pnpm install
```

4. Construir todo el código:
```bash
pnpm build
```
Nota: Si aparece el error "Exit code 134 (JavaScript heap out of memory)", ajustar la memoria heap de Node.js

5. Iniciar la aplicación:
```bash
pnpm start
```

### Actualizar Instalación Local
1. Navegar al directorio donde está instalado Flowise
2. Descargar las últimas actualizaciones del repositorio de GitHub:
```bash
git pull origin main
```

3. Instalar dependencias actualizadas y reconstruir el proyecto:
```bash
pnpm install
pnpm build
```

4. Iniciar la aplicación:
```bash
pnpm start
```

Nota: Este método asegura que tu instalación local se actualice con todas las nuevas características y correcciones.

## Chains

Una chain es como una memoria ordenada que guarda la conversación entre tú y la IA.

Imagina una cadena de metal, donde cada eslabón representa un momento de la conversación. Igual que los eslabones de una cadena están conectados uno tras otro y cada uno se apoya en el anterior, en una chain de IA:

- Cada eslabón nuevo (mensaje) se conecta con el anterior
- Los eslabones están unidos en orden (la conversación fluye en secuencia)
- Cada eslabón sostiene al siguiente (cada mensaje da contexto al siguiente)
- La cadena entera da fuerza al conjunto (toda la conversación da contexto completo)

![Diagrama de Chain](/../../.gitbook/assets/partes/parte1/Chains.Diagrama.png)

## Chat Models

Un Chat Model es el nodo que nos da acceso a diferentes modelos de inteligencia artificial. Es como una puerta de entrada: si queremos usar ChatGPT, Gemini, Claude o cualquier otro modelo de IA, necesitamos usar este nodo.
Es prácticamente igual que un nodo LLM, pero con algunas ventajas:

- Podemos elegir entre más modelos diferentes
- Incluye alternativas gratuitas como Gemini o Cerebras

Y si lo conectamos con el concepto de chain que vimos antes:

La chain mantiene el historial de la conversación
El Chat Model es el nodo que toma esa conversación y la envía al modelo de IA que hayamos elegido
El modelo nos devuelve una respuesta que se añade como un nuevo eslabón a la chain

Por eso es un nodo que SIEMPRE necesitaremos en nuestros proyectos de Flowise: es literalmente el punto de conexión con la IA. Sin él, no podríamos obtener respuestas de ningún modelo de inteligencia artificial.

![Chat Model](/../../.gitbook/assets/partes/parte1/ChatModel.png)

## Memoria

La memoria es lo que permite que la IA "recuerde" las conversaciones previas. Es como si la IA llevara un diario de lo que habla con cada persona.
Imagina esta situación:

Tú: "Hola, me llamo Ana"
IA: "¡Hola Ana! ¿En qué puedo ayudarte?"
Tú: "¿Cómo me llamo?"
IA: "Te llamas Ana, me lo dijiste antes"

¿Cómo logra la IA recordar esto? A través del sessionID.
El sessionID es como un DNI para cada conversación. Cuando alguien nuevo comienza a chatear, Flowise le asigna automáticamente un sessionID único. Es como darle a cada persona su propia libreta donde se guarda solo su conversación.
Hay varias formas de guardar estas "libretas de conversación":

- En la memoria temporal (como Buffer Memory)
- En bases de datos (como MongoDB o DynamoDB)
- En servicios específicos (como Redis o Zep)

Lo importante es entender que:

Cada usuario tiene su propia conversación separada
El sessionID es lo que permite mantener las conversaciones separadas
En la interfaz de usuario, esto se maneja automáticamente
Si usas la API, puedes especificar tu propio sessionID para mantener conversaciones separadas

Todo esto se puede ver y gestionar desde la interfaz de Flowise, donde puedes revisar las conversaciones pasadas, como si tuvieras acceso a todas las "libretas" de conversación.

La [memoria](../../integraciones/langchain/memory/README.md) es crucial para mantener el contexto en las conversaciones.

![Diagrama Memoria Simplificado](/../../.gitbook/assets/partes/parte1/Memoria1.png)

## Cómo estar al día con Flowise

Para estar siempre actualizado con las últimas novedades y cambios de Flowise, tienes dos recursos principales:

### 1. Repositorio Principal de Flowise:

Visita https://github.com/FlowiseAI/Flowise
- Aquí encontrarás el código fuente completo
- Puedes ver las últimas actualizaciones y contribuciones
- Puedes marcar el repositorio con una estrella para seguirlo fácilmente


### 2. Sección de Releases:

Visita https://github.com/FlowiseAI/Flowise/releases

Cada release incluye:

- Número de versión
- Lista detallada de cambios y mejoras
- Nuevas funcionalidades añadidas
- Correcciones de errores
- Fecha de publicación

Es recomendable revisar periódicamente la sección de releases para:

- Conocer las nuevas funcionalidades disponibles
- Estar al tanto de mejoras en el rendimiento
- Saber qué errores se han corregido
- Planificar cuándo actualizar tu instalación de Flowise