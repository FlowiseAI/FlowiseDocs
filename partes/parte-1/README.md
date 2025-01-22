# Parte 1: Introducción

En esta primera parte del curso, aprenderás los fundamentos esenciales para comenzar con Flowise. Cubriremos desde la instalación básica hasta conceptos fundamentales que necesitarás para crear tus primeros chatflows.

## Contenidos
- [Instalación de Flowise](#instalación-de-flowise)
- Interfaz de Usuario
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

![Diagrama de Chain](/../../FlowiseDocs/.gitbook/assets/partes/parte1/Chains.Diagrama.png)

## Chat Models

Los [Chat Models](../../documentacion-oficial/usar-flowise/chat-models.md) son el núcleo de la funcionalidad conversacional.

### Modelos Disponibles
- [ChatGPT](../../integraciones/langchain/chat-models/chatopenai.md)
- [Google AI](../../integraciones/langchain/chat-models/google-ai.md)
- [Anthropic Claude](../../integraciones/langchain/chat-models/chatanthropic.md)
- [Ollama](../../integraciones/langchain/chat-models/chatollama.md)

## Cómo estar al día con Flowise

### Proceso de Actualización
```bash
git pull origin main
pnpm install
pnpm build
```

### Buenas Prácticas
- Revisar el [changelog](https://github.com/FlowiseAI/Flowise/releases) antes de actualizar
- Hacer backup de tus flujos importantes
- Probar en un entorno de desarrollo primero

## Memoria

La [memoria](../../integraciones/langchain/memory/README.md) es crucial para mantener el contexto en las conversaciones.

### Tipos de Memoria
- [Buffer Memory](../../integraciones/langchain/memory/buffer-memory.md)
- [Buffer Window Memory](../../integraciones/langchain/memory/buffer-window-memory.md)
- [Conversation Summary Memory](../../integraciones/langchain/memory/conversation-summary-memory.md)

### Consideraciones
- Gestión del contexto
- Límites de tokens
- Persistencia de datos
- Optimización de recursos

## Próximos Pasos

Al completar esta parte, estarás preparado para:
- Crear flujos básicos funcionales
- Trabajar con diferentes modelos de chat
- Mantener tu instalación actualizada
- Gestionar la memoria en tus aplicaciones
- Avanzar hacia conceptos más avanzados en la [Parte 2: Chains Avanzadas](../parte-2/README.md) 