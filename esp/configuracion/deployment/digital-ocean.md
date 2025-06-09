---
description: Learn how to deploy Flowise on Digital Ocean
---

# DigitalOcean

## Crear un Droplet

1. Haz click en **Create** y selecciona **Droplets**

<figure><img src="../../.gitbook/assets/image (8) (2).png" alt=""><figcaption></figcaption></figure>

2. Selecciona Ubuntu y el plan que prefieras

<figure><img src="../../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>

3. Selecciona un datacenter region

<figure><img src="../../.gitbook/assets/image (11) (2).png" alt=""><figcaption></figcaption></figure>

4. Crea un nuevo SSH key o selecciona uno existente

<figure><img src="../../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

5. Haz click en **Create Droplet**

## Conectarse al Droplet

Para Windows, sigue esta [guía](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/putty/).

Para Mac/Linux, sigue esta [guía](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/openssh/).

## Instalar Docker

1. ```bash
curl -fsSL https://get.docker.com -o get-docker.sh
```
2. ```bash
sudo sh get-docker.sh
```
3. Instala docker-compose:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

4. Establece los permisos:

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

## Setup

1. Clona el repositorio

```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2. Ingresa al directorio docker

```bash
cd Flowise && cd docker
```

3. Crea un archivo `.env`. Puedes usar tu editor favorito. Yo usaré `nano`

```bash
nano .env
```

<figure><img src="../../.gitbook/assets/image (10) (2).png" alt="" width="375"><figcaption></figcaption></figure>

4. Especifica las environment variables:

```sh
PORT=3000
DATABASE_PATH=/root/.flowise
SECRETKEY_PATH=/root/.flowise
LOG_PATH=/root/.flowise/logs
BLOB_STORAGE_PATH=/root/.flowise/storage
```

5. Inicia los contenedores

```bash
docker compose up -d
```

6. Verifica que los contenedores estén corriendo

```bash
docker ps
```

7. Abre tu navegador y navega a `http://[IP_ADDRESS]:3000`

## Configurar Nginx

### Paso 1 — Instalar y Verificar Nginx

1. Actualiza el índice de paquetes:

```bash
sudo apt update
```

2. Instala Nginx:

```bash
sudo apt install nginx
```

3. Verifica que Nginx esté corriendo:

```bash
sudo systemctl status nginx
```

Deberías ver una salida similar a:

```bash
● nginx.service - A high performance web server and a reverse proxy server
    Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
    Active: active (running) since Mon 2022-08-29 06:52:46 UTC; 39min ago
      Docs: man:nginx(8)
  Main PID: 9919 (nginx)
     Tasks: 2 (limit: 2327)
    Memory: 2.9M
       CPU: 50ms
    CGroup: /system.slice/nginx.service
            ├─9919 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
            └─9920 "nginx: worker process
```

A continuación, agregarás un custom server block con tu dominio y proxy del app server.

### Paso 2 — Configurando tu Server Block + DNS Record

Es una práctica recomendada crear un archivo de configuración personalizado para tus nuevas adiciones de server block, en lugar de editar la configuración por defecto directamente.

1. Crea y abre un nuevo archivo de configuración de Nginx usando nano o tu editor de texto preferido:

```bash
sudo nano /etc/nginx/sites-available/your_domain
```

2. Inserta lo siguiente en tu nuevo archivo, asegurándote de reemplazar `your_domain` con tu propio nombre de dominio:

```
server {
    listen 80;
    listen [::]:80;
    server_name your_domain; #Example: demo.flowiseai.com
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_cache_bypass $http_upgrade;
    }
}
```

3. Guarda y sale, con `nano` puedes hacerlo presionando `CTRL+O` y luego `CTRL+X`.
4. Luego, habilita este archivo de configuración creando un enlace desde el directorio sites-enabled que Nginx lee al inicio, asegúrate de reemplazar `your_domain` con tu propio nombre de dominio:

```bash
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

5. Ahora puedes probar tu archivo de configuración para errores de sintaxis:

```bash
sudo nginx -t
```

6. Con problemas reportados, reinicia Nginx para aplicar tus cambios:

```bash
sudo systemctl restart nginx
```

7. Ve a tu proveedor de DNS y agrega un nuevo registro A. El nombre será tu nombre de dominio y el valor será la dirección IP pública de tu droplet

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt="" width="367"><figcaption></figcaption></figure>

Nginx ahora está configurado como proxy inverso para tu servidor de aplicaciones. Ahora deberías poder abrir la aplicación: http://yourdomain.com.

### Paso 3 — Instalar Certbot para HTTPS (SSL)

Si quieres agregar una conexión `https` segura a tu droplet como https://yourdomain.com, necesitarás hacer lo siguiente:

1. Para instalar Certbot y habilitar HTTPS en NGINX, nos apoyaremos en Python. Así que, primero de todo, vamos a configurar un entorno virtual:

```bash
apt install python3.10-venv
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
```

2. Después, ejecuta este comando para instalar Certbot:

```bash
sudo /opt/certbot/bin/pip install certbot certbot-nginx
```

3. Ahora, ejecuta este comando para asegurarte de que el comando `certbot` pueda ser ejecutado:

```bash
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

4. Finalmente, ejecuta este comando para obtener un certificado y permitir que Certbot modifique automáticamente la configuración de NGINX, habilitando HTTPS:

```bash
sudo certbot --nginx
```

5. Después de seguir el asistente de generación de certificados, deberías poder acceder a tu droplet a través de HTTPS usando la dirección https://yourdomain.com

### Configurar renovación automática

Para permitir que Certbot renueve automáticamente los certificados, basta con agregar una tarea cron ejecutando el siguiente comando:

```bash
echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

## ¡Felicidades!

Has configurado correctamente Flowise en tu droplet, con certificado SSL en tu dominio [🥳](https://emojipedia.org/partying-face/)

## Pasos para actualizar Flowise en Digital Ocean

1. Navega al directorio donde instalaste flowise

```bash
cd Flowise/docker
```

2. Detén y elimina la imagen docker

Nota: Esto no eliminará tus flujos ya que la base de datos se almacena en una carpeta separada

```bash
sudo docker compose stop
sudo docker compose rm
```

3. Extrae la imagen de Flowise más reciente

Puedes verificar la última versión de lanzamiento [aquí](https://github.com/FlowiseAI/Flowise/releases)

```bash
docker pull flowiseai/flowise
```

4. Inicia el contenedor docker

```bash
docker compose up -d
```
