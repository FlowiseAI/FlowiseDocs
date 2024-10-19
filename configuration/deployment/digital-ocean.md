---
description: Learn how to deploy Flowise on Digital Ocean
---

# Digital Ocean

***

## Create Droplet

In this section, we are going to create a Droplet. For more information, refer to [official guide](https://docs.digitalocean.com/products/droplets/quickstart/).

1. First, Click **Droplets** from the dropdown

<figure><img src="../../.gitbook/assets/image (15) (2).png" alt=""><figcaption></figcaption></figure>

2. Select Data Region and a Basic $6/mo Droplet type

<figure><img src="../../.gitbook/assets/image (17) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. Select Authentication Method. In this example, we are going to use Password

<figure><img src="../../.gitbook/assets/image (5) (2).png" alt=""><figcaption></figcaption></figure>

4. After a while you should be able to see your droplet created successfully

<figure><img src="../../.gitbook/assets/image (7) (2) (1).png" alt=""><figcaption></figcaption></figure>

## How to Connect to your Droplet

For Windows follow this [guide](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/putty/).

For Mac/Linux, follow this [guide](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/openssh/).

## Install Docker

1. ```
   curl -fsSL https://get.docker.com -o get-docker.sh
   ```
2. ```
   sudo sh get-docker.sh
   ```
3. Install docker-compose:

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

4. Set permission:

```
sudo chmod +x /usr/local/bin/docker-compose
```

## Setup

1. Clone the repo

```
git clone https://github.com/FlowiseAI/Flowise.git
```

2. Cd into docker folder

```bash
cd Flowise && cd docker
```

3. Create a `.env` file. You can use your favourite editor. I'll use `nano`

```bash
nano .env
```

<figure><img src="../../.gitbook/assets/image (10) (2).png" alt="" width="375"><figcaption></figcaption></figure>

4. Specify the env variables:

```sh
PORT=3000
DATABASE_PATH=/root/.flowise
APIKEY_PATH=/root/.flowise
SECRETKEY_PATH=/root/.flowise
LOG_PATH=/root/.flowise/logs
BLOB_STORAGE_PATH=/root/.flowise/storage
```

4. _(Optional)_ You can also specify `FLOWISE_USERNAME` and `FLOWISE_PASSWORD` for app level authorization. See more [broken-reference](broken-reference/ "mention")
5. Then press `Ctrl + X` to Exit, and `Y` to save the file
6. Run docker compose

```bash
docker compose up -d
```

7. You can then view the app: "Your Public IPv4 DNS":3000. Example: `176.63.19.226:3000`
8. You can bring the app down by:

```bash
docker compose stop
```

9. You can pull from latest image by:

```bash
docker pull flowiseai/flowise
```

## Adding Reverse Proxy & SSL

A reverse proxy is the recommended method to expose an application server to the internet. It will let us connect to our droplet using a URL alone instead of the server IP and port number. This provides security benefits in isolating the application server from direct internet access, the ability to centralize firewall protection, a minimized attack plane for common threats such as denial of service attacks, and most importantly for our purposes, the ability to terminate SSL/TLS encryption in a single place.

> A lack of SSL on your Droplet will cause the embeddable widget and API endpoints to be inaccessible in modern browsers. This is because browsers have begun to deprecate HTTP in favor of HTTPS, and block HTTP requests from pages loaded over HTTPS.

### Step 1 — Installing Nginx

1. Nginx is available for installation with apt through the default repositories. Update your repository index, then install Nginx:

```bash
sudo apt update
sudo apt install nginx
```

> Press Y to confirm the installation. If you are asked to restart services, press ENTER to accept the defaults.

2. You need to allow access to Nginx through your firewall. Having set up your server according to the initial server prerequisites, add the following rule with ufw:

```bash
sudo ufw allow 'Nginx HTTP'
```

3. Now you can verify that Nginx is running:

```bash
systemctl status nginx
```

Output:

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

Next you will add a custom server block with your domain and app server proxy.

### Step 2 — Configuring your Server Block + DNS Record

It is recommended practice to create a custom configuration file for your new server block additions, instead of editing the default configuration directly.

1. Create and open a new Nginx configuration file using nano or your preferred text editor:

```bash
sudo nano /etc/nginx/sites-available/your_domain
```

2. Insert the following into your new file, making sure to replace `your_domain` with your own domain name:

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

3. Save and exit, with `nano` you can do this by hitting `CTRL+O` then `CTRL+X`.
4. Next, enable this configuration file by creating a link from it to the sites-enabled directory that Nginx reads at startup, making sure again to replace `your_domain` with your own domain name::

```bash
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

5. You can now test your configuration file for syntax errors:

```bash
sudo nginx -t
```

6. With no problems reported, restart Nginx to apply your changes:

```bash
sudo systemctl restart nginx
```

7. Go to your DNS provider, and add a new A record. Name will be your domain name, and value will be the Public IPv4 address from your droplet

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt="" width="367"><figcaption></figcaption></figure>

Nginx is now configured as a reverse proxy for your application server. You should now be able to open the app: http://yourdomain.com.

### Step 3 — Installing Certbot for HTTPS (SSL)

If you'd like to add a secure `https` connection to your Droplet like https://yourdomain.com, you'll need to do the following:

1. For installing Certbot and enabling HTTPS on NGINX, we will rely on Python. So, first of all, let's set up a virtual environment:

```bash
apt install python3.10-venv
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
```

2. Afterwards, run this command to install Certbot:

```bash
sudo /opt/certbot/bin/pip install certbot certbot-nginx
```

3. Now, execute the following command to ensure that the `certbot` command can be run:

```bash
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

4. Finally, run the following command to obtain a certificate and let Certbot automatically modify the NGINX configuration, enabling HTTPS:

```bash
sudo certbot --nginx
```

5. After following the certificate generation wizard, we will be able to access our Droplet via HTTPS using the address https://yourdomain.com

### Set up automatic renewal

To enable Certbot to automatically renew the certificates, it is sufficient to add a cron job by running the following command:

```bash
echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

## Congratulations!

You have successfully setup Flowise on your Droplet, with SSL certificate on your domain [🥳](https://emojipedia.org/partying-face/)

## Steps to update Flowise on Digital Ocean

1. Navigate to the directory you installed flowise in

```bash
cd Flowise/docker
```

2. Stop and remove docker image

Note: This will not delete your flows as the database is stored in a separate folder

```bash
sudo docker compose stop
sudo docker compose rm
```

3. Pull the latest Flowise Image

You can check the latest version release [here](https://github.com/FlowiseAI/Flowise/releases)

```bash
docker pull flowiseai/flowise
```

4. Start the docker

```bash
docker compose up -d
```
