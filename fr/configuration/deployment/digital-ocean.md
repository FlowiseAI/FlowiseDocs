---
description: Apprenez à déployer Flowise sur Digital Ocean
---

# Digital Ocean

***

## Créer un Droplet

Dans cette section, nous allons créer un Droplet. Pour plus d'informations, consultez le [guide officiel](https://docs.digitalocean.com/products/droplets/quickstart/).

1. Tout d'abord, cliquez sur **Droplets** dans le menu déroulant

<figure><img src="../../.gitbook/assets/image (15) (2) (2).png" alt=""><figcaption></figcaption></figure>

2. Sélectionnez la région de données et un type de Droplet Basique à 6 $/mois

<figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. Sélectionnez la méthode d'authentification. Dans cet exemple, nous allons utiliser un mot de passe

<figure><img src="../../.gitbook/assets/image (5) (2).png" alt=""><figcaption></figcaption></figure>

4. Après un moment, vous devriez voir votre Droplet créé avec succès

<figure><img src="../../.gitbook/assets/image (7) (2) (1).png" alt=""><figcaption></figcaption></figure>

## Comment se connecter à votre Droplet

Pour Windows, suivez ce [guide](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/putty/).

Pour Mac/Linux, suivez ce [guide](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/openssh/).

## Installer Docker

1. ```
   curl -fsSL https://get.docker.com -o get-docker.sh   ```
2. ```
   sudo sh get-docker.sh
   ```
3. Installer docker-compose :

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

4. Définir les autorisations :

```
sudo chmod +x /usr/local/bin/docker-compose
```

## Configuration

1. Clone le dépôt

```
git clone https://github.com/FlowiseAI/Flowise.git
```

2. Accédez au dossier docker

```bash
cd Flowise && cd docker
```

3. Créez un fichier `.env`. Vous pouvez utiliser votre éditeur préféré. J'utiliserai `nano`

```bash
nano .env
```

<figure><img src="../../.gitbook/assets/image (10) (2) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. Spécifiez les variables d'environnement :

```sh
PORT=3000
DATABASE_PATH=/root/.flowise
SECRETKEY_PATH=/root/.flowise
LOG_PATH=/root/.flowise/logs
BLOB_STORAGE_PATH=/root/.flowise/storage
```

5. Ensuite, appuyez sur `Ctrl + X` pour quitter, puis sur `Y` pour enregistrer le fichier  
6. Exécutez docker compose

```bash
docker compose up -d
```

7. Vous pouvez ensuite visualiser l'application : "Votre DNS IPv4 public" :3000. Exemple : `176.63.19.226:3000`
8. Vous pouvez arrêter l'application en :

```bash
docker compose stop
```

9. Vous pouvez récupérer la dernière image en :

```bash
docker pull flowiseai/flowise
```

## Ajout d'un Proxy Inverse & SSL

Un proxy inverse est la méthode recommandée pour exposer un serveur d'application à Internet. Il nous permettra de nous connecter à notre droplet en utilisant uniquement une URL au lieu de l'adresse IP du serveur et du numéro de port. Cela offre des avantages en matière de sécurité en isolant le serveur d'application de l'accès direct à Internet, la possibilité de centraliser la protection du pare-feu, un plan d'attaque minimisé pour les menaces courantes telles que les attaques par déni de service, et surtout pour nos besoins, la capacité de terminer le chiffrement SSL/TLS à un seul endroit.

> A lack of SSL on your Droplet will cause the embeddable widget and API endpoints to be inaccessible in modern browsers. This is because browsers have begun to deprecate HTTP in favor of HTTPS, and block HTTP requests from pages loaded over HTTPS.

### Étape 1 — Installation de Nginx

1. Nginx est disponible pour installation avec apt via les dépôts par défaut. Mettez à jour votre index de dépôts, puis installez Nginx :

```bash
sudo apt update
sudo apt install nginx
```

> Press Y to confirm the installation. If you are asked to restart services, press ENTER to accept the defaults.

```markdown
2. Vous devez autoriser l'accès à Nginx via votre pare-feu. Après avoir configuré votre serveur selon les prérequis initiaux, ajoutez la règle suivante avec ufw :
```

```bash
sudo ufw allow 'Nginx HTTP'
```

3. Maintenant, vous pouvez vérifier que Nginx fonctionne :

```bash
systemctl status nginx
```

Sure, please provide the Markdown chunk you would like me to translate into French.

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

### Étape 2 — Configuration de votre bloc serveur + enregistrement DNS

Il est recommandé de créer un fichier de configuration personnalisé pour vos nouvelles additions de bloc serveur, plutôt que de modifier directement la configuration par défaut.

1. Créez et ouvrez un nouveau fichier de configuration Nginx en utilisant nano ou votre éditeur de texte préféré :

```bash
sudo nano /etc/nginx/sites-available/your_domain
```

2. Insérez ce qui suit dans votre nouveau fichier, en veillant à remplacer `your_domain` par le nom de votre propre domaine :

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

3. Enregistrez et quittez, avec `nano` vous pouvez le faire en appuyant sur `CTRL+O` puis `CTRL+X`.  
4. Ensuite, activez ce fichier de configuration en créant un lien depuis celui-ci vers le répertoire sites-enabled que Nginx lit au démarrage, en vous assurant encore une fois de remplacer `your_domain` par votre propre nom de domaine :

```bash
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

5. Vous pouvez maintenant tester votre fichier de configuration pour détecter des erreurs de syntaxe :

```bash
sudo nginx -t
```

6. Sans problèmes signalés, redémarrez Nginx pour appliquer vos modifications :

```bash
sudo systemctl restart nginx
```

7. Allez chez votre fournisseur DNS et ajoutez un nouvel enregistrement A. Le nom sera votre nom de domaine, et la valeur sera l'adresse IPv4 publique de votre droplet.

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt="" width="367"><figcaption></figcaption></figure>

Nginx est maintenant configuré comme un proxy inverse pour votre serveur d'application. Vous devriez maintenant pouvoir ouvrir l'application : http://yourdomain.com.

### Étape 3 — Installation de Certbot pour HTTPS (SSL)

Si vous souhaitez ajouter une connexion sécurisée `https` à votre Droplet comme https://yourdomain.com, vous devrez faire ce qui suit :

1. Pour installer Certbot et activer HTTPS sur NGINX, nous allons nous appuyer sur Python. Donc, tout d'abord, configurons un environnement virtuel :

```bash
apt install python3.10-venv
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
```

2. Ensuite, exécutez cette commande pour installer Certbot :

```bash
sudo /opt/certbot/bin/pip install certbot certbot-nginx
```

3. Maintenant, exécutez la commande suivante pour vous assurer que la commande `certbot` peut être exécutée :

```bash
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

4. Enfin, exécutez la commande suivante pour obtenir un certificat et permettre à Certbot de modifier automatiquement la configuration NGINX, activant ainsi HTTPS :

```bash
sudo certbot --nginx
```

5. Après avoir suivi l'assistant de génération de certificat, nous pourrons accéder à notre Droplet via HTTPS en utilisant l'adresse https://yourdomain.com

### Configurer le renouvellement automatique

Pour permettre à Certbot de renouveler automatiquement les certificats, il suffit d'ajouter une tâche cron en exécutant la commande suivante :

```bash
echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

## Félicitations !

Vous avez réussi à configurer Flowise sur votre Droplet, avec un certificat SSL sur votre domaine [🥳](https://emojipedia.org/partying-face/)

## Étapes pour mettre à jour Flowise sur Digital Ocean

1. Accédez au répertoire dans lequel vous avez installé flowise

```bash
cd Flowise/docker
```

2. Arrêter et supprimer l'image docker

Remarque : Cela ne supprimera pas vos flux car la base de données est stockée dans un dossier séparé

```bash
sudo docker compose stop
sudo docker compose rm
```

3. Récupérez la dernière image Flowise

Vous pouvez consulter la dernière version publiée [ici](https://github.com/FlowiseAI/Flowise/releases)

```bash
docker pull flowiseai/flowise
```

4. Démarrer le docker

```bash
docker compose up -d
```
