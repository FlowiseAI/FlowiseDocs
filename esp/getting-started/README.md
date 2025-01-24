# Empezando

***

## Cloud

El auto-hospedaje requiere habilidades técnicas para configurar la instancia, respaldar la base de datos y mantener actualizaciones. Si no tienes experiencia administrando servidores y solo quieres usar la aplicación web, te recomendamos usar [Flowise Cloud](https://flowiseai.com/join).

## Inicio Rápido

{% hint style="info" %}
Pre-requisito: asegúrate de que [NodeJS](https://nodejs.org/en/download) esté instalado en tu máquina. Node `v18.15.0` o `v20` y superiores son compatibles.
{% endhint %}

Instala Flowise localmente usando NPM.

1. Instala Flowise:

```bash
npm install -g flowise
```

También puedes instalar una versión específica. Consulta las [versiones disponibles](https://www.npmjs.com/package/flowise?activeTab=versions).

```bash
npm install -g flowise@x.x.x
```

2. Inicia Flowise:

```bash
npx flowise start
```

3. Abre: [http://localhost:3000](http://localhost:3000)

***

## Docker

Hay dos formas de implementar Flowise con Docker:

### Docker Compose

1. Ve a la carpeta `docker` en la raíz del proyecto.  
2. Copia el archivo `.env.example` y pégalo como otro archivo llamado `.env`.  
3. Ejecuta:  

```bash
docker compose up -d
```

4. Abre: [http://localhost:3000](http://localhost:3000)  
5. Puedes detener los contenedores ejecutando:  

```bash
docker compose stop
```

### Imagen de Docker

1. Crea la imagen:  

```bash
docker build --no-cache -t flowise .
```

2. Ejecuta la imagen:  

```bash
docker run -d --name flowise -p 3000:3000 flowise
```

3. Detén la imagen:  

```bash
docker stop flowise
```

***

## Para Desarrolladores

Flowise tiene 3 módulos diferentes en un solo repositorio monolítico:

* **Servidor**: Backend en Node para servir la lógica de API.  
* **UI**: Frontend en React.  
* **Componentes**: Componentes de integración.  

### Prerrequisito

Instala [PNPM](https://pnpm.io/installation).

```bash
npm i -g pnpm
```

### Configuración 1

Configuración simple usando PNPM:

1. Clona el repositorio:  

```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2. Entra en la carpeta del repositorio:  

```bash
cd Flowise
```

3. Instala todas las dependencias de todos los módulos:  

```bash
pnpm install
```

4. Construye el código:  

```bash
pnpm build
```

Inicia la app en [http://localhost:3000](http://localhost:3000)

```bash
pnpm start
```

### Configuración 2

Configuración paso a paso para contribuidores del proyecto:

1. Haz un fork del [Repositorio Oficial de Flowise](https://github.com/FlowiseAI/Flowise).  
2. Clona tu repositorio forked.  
3. Crea una nueva rama, consulta la [guía](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository). Convenciones de nombres:  
   * Para rama de funcionalidades: `feature/<NuevaFuncionalidad>`.  
   * Para rama de corrección de errores: `bugfix/<NuevaCorrección>`.  
4. Cambia a la rama que acabas de crear.  
5. Entra en la carpeta del repositorio:  

```bash
cd Flowise
```

6. Instala todas las dependencias de todos los módulos:  

```bash
pnpm install
```

7. Construye el código:  

```bash
pnpm build
```

8. Inicia la app en [http://localhost:3000](http://localhost:3000)  

```bash
pnpm start
```

9. Para un entorno de desarrollo:  

* Crea un archivo `.env` y especifica el `PORT` (consulta `.env.example`) en `packages/ui`.  
* Crea un archivo `.env` y especifica el `PORT` (consulta `.env.example`) en `packages/server`.  

```bash
pnpm dev
```

* Los cambios realizados en `packages/ui` o `packages/server` se reflejarán en [http://localhost:8080](http://localhost:8080/).  
* Para los cambios realizados en `packages/components`, deberás reconstruir para reflejar los cambios:  

    ```bash
    pnpm build
    ```

    y luego:  

    ```bash
    pnpm start
    ```

    para asegurarte de que todo funcione correctamente en producción.  

***

## Para Empresas

Los planes empresariales tienen repositorios e imágenes Docker separados.  

Una vez que se te haya otorgado acceso a ambos, la configuración es la misma que [#setup-1](./#setup-1 "mention"). Antes de iniciar la aplicación, los usuarios empresariales deben completar los valores de los parámetros empresariales en el archivo `.env`. Consulta `.env.example` para los cambios requeridos.  

Contacta a support@flowiseai.com para obtener los valores de las siguientes variables de entorno:  

```
LICENSE_URL
FLOWISE_EE_LICENSE_KEY
```

Para la instalación con Docker:  

```bash
cd docker
cd enterprise
docker compose up -d
```

***

## Aprende Más

En este tutorial en video, Leon proporciona una introducción a Flowise y explica cómo configurarlo en tu máquina local.  

{% embed url="https://youtu.be/nqAK_L66sIQ" %}

## Guía Comunitaria

* [Introducción a \[Práctica\] Construcción de Aplicaciones LLM con Flowise / LangChain](https://volcano-ice-cd6.notion.site/Introduction-to-Practical-Building-LLM-Applications-with-Flowise-LangChain-03d6d75bfd20495d96dfdae964bea5a5)  
* [Flowise / LangChainによるLLMアプリケーション構築\[実践\]入門](https://volcano-ice-cd6.notion.site/Flowise-LangChain-LLM-e106bb0f7e2241379aad8fa428ee064a)
