---
description: Apprenez à déployer Flowise sur AWS
---

# AWS

***

## Prérequis

Cela nécessite une compréhension de base du fonctionnement d'AWS.

Deux options sont disponibles pour déployer Flowise sur AWS :

* [Déployer sur ECS en utilisant CloudFormation](aws.md#deploy-on-ecs-using-cloudformation)
* [Configurer manuellement une instance EC2](aws.md#launch-ec2-instance)

## Déployer sur ECS en utilisant CloudFormation

Le modèle CloudFormation est disponible ici : [https://gist.github.com/MrHertal/549b31a18e350b69c7200ae8d26ed691](https://gist.github.com/MrHertal/549b31a18e350b69c7200ae8d26ed691)

Il déploie Flowise sur un cluster ECS exposé via ELB.

Il a été inspiré par cette architecture de référence : [https://github.com/aws-samples/ecs-refarch-cloudformation](https://github.com/aws-samples/ecs-refarch-cloudformation)

N'hésitez pas à modifier ce modèle pour adapter des éléments tels que la version de l'image Flowise, les variables d'environnement, etc.

Exemple de commande pour déployer Flowise en utilisant le [CLI AWS](https://aws.amazon.com/fr/cli/) :

```bash
aws cloudformation create-stack --stack-name flowise --template-body file://flowise-cloudformation.yml --capabilities CAPABILITY_IAM
```

Après le déploiement, l'URL de votre application Flowise est disponible dans les sorties de la pile CloudFormation.

## Déployer sur ECS en utilisant Terraform

Les fichiers Terraform (`variables.tf`, `main.tf`) sont disponibles dans ce dépôt GitHub : [terraform-flowise-setup](https://github.com/huiseo/terraform-flowise-setup/tree/main).

Cette configuration déploie Flowise sur un cluster ECS exposé via un Application Load Balancer (ALB). Elle est basée sur les meilleures pratiques AWS pour les déploiements ECS.

Vous pouvez modifier le modèle Terraform pour ajuster :

* La version de l'image Flowise
* Les variables d'environnement
* Les configurations des ressources (CPU, mémoire, etc.)

### Exemples de commandes pour le déploiement :

1. **Initialiser Terraform :**

```bash
terraform init
terraform apply
terraform destroy
```
```markdown
## Lancer une instance EC2

1. Dans le tableau de bord EC2, cliquez sur **Lancer une instance**

<figure><img src="../../.gitbook/assets/image (19) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Faites défiler vers le bas et **Créez une nouvelle paire de clés** si vous n'en avez pas

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

3. Remplissez le nom de la paire de clés de votre choix. Pour Windows, nous utiliserons `.ppk` et PuTTY pour nous connecter à l'instance. Pour Mac et Linux, nous utiliserons `.pem` et OpenSSH

<figure><img src="../../.gitbook/assets/image (15) (2) (1).png" alt="" width="370"><figcaption></figcaption></figure>

4. Cliquez sur **Créer une paire de clés** et sélectionnez un chemin d'emplacement pour enregistrer le fichier `.ppk`
5. Ouvrez la barre latérale gauche et ouvrez un nouvel onglet à partir de **Groupes de sécurité**. Ensuite, **Créez un groupe de sécurité**

<figure><img src="../../.gitbook/assets/image (20) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. Remplissez le nom et la description de votre groupe de sécurité. Ensuite, ajoutez ce qui suit aux Règles entrantes et **Créez un groupe de sécurité**

<figure><img src="../../.gitbook/assets/image (12) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

7. Retournez au premier onglet (Lancer une instance EC2) et faites défiler vers le bas jusqu'à **Paramètres réseau**. Sélectionnez le groupe de sécurité que vous venez de créer

<figure><img src="../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

8. Cliquez sur **Lancer l'instance**. Revenez au tableau de bord EC2, après quelques minutes, nous devrions voir une nouvelle instance opérationnelle [🎉](https://emojipedia.org/party-popper/)

<figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Comment se connecter à votre instance (Windows)

1. Pour Windows, nous allons utiliser PuTTY. Vous pouvez le télécharger [ici](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).
2. Ouvrez PuTTY et remplissez le **HostName** avec le nom DNS IPv4 public de votre instance

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. Dans la barre latérale gauche de la configuration de PuTTY, développez **SSH** et cliquez sur **Auth**. Cliquez sur Parcourir et sélectionnez le fichier `.ppk` que vous avez téléchargé précédemment.

<figure><img src="../../.gitbook/assets/image (23) (1) (1).png" alt="" width="296"><figcaption></figcaption></figure>

4. Cliquez sur **Ouvrir** et **Acceptez** le message contextuel

<figure><img src="../../.gitbook/assets/image (18) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

5. Ensuite, connectez-vous en tant que `ec2-user`

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

6. Vous êtes maintenant connecté à l'instance EC2

## Comment se connecter à votre instance (Mac et Linux)

1. Ouvrez l'application Terminal sur votre Mac/Linux.
2. _(Optionnel)_ Modifiez les permissions du fichier de clé privée pour restreindre l'accès :
``````bash
chmod 400 /path/to/mykey.pem
```

3. Utilisez la commande `ssh` pour vous connecter à votre instance EC2, en spécifiant le nom d'utilisateur (`ec2-user`), le DNS public IPv4 et le chemin vers le fichier `.pem`.

```bash
ssh -i /Users/username/Documents/mykey.pem ec2-user@ec2-123-45-678-910.compute-1.amazonaws.com
```

4. Appuyez sur Entrée, et si tout est configuré correctement, vous devriez établir avec succès une connexion SSH à votre instance EC2.

## Installer Docker

1. Appliquez les mises à jour en attente en utilisant la commande yum :

```bash
sudo yum update
```

2. Recherchez le paquet Docker :

```bash
sudo yum search docker
```

3. Obtenez des informations sur la version :

```bash
sudo yum info docker
```

4. Installez Docker, exécutez :

```bash
sudo yum install docker
```

5. Ajoutez l'appartenance au groupe pour l'utilisateur par défaut ec2-user afin de pouvoir exécuter toutes les commandes docker sans utiliser la commande sudo :

```bash
sudo usermod -a -G docker ec2-user
id ec2-user
newgrp docker
```

6. Installer docker-compose :

```bash
sudo yum install docker-compose-plugin
```

7. Activer le service docker au démarrage de l'AMI :

```bash
sudo systemctl enable docker.service
```

8. Démarrez le service Docker :

```bash
sudo systemctl start docker.service
```

## Installer Git

```bash
sudo yum install git -y
```

## Configuration

1. Clone le dépôt

```bash
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

<figure><img src="../../.gitbook/assets/image (13) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

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

7. Votre application est maintenant prête à l'adresse DNS IPv4 publique sur le port 3000 :

```
http://ec2-123-456-789.compute-1.amazonaws.com:3000
```

8. Vous pouvez fermer l'application en :

```bash
docker compose stop
```

9. Vous pouvez récupérer la dernière image en :

```bash
docker pull flowiseai/flowise
```
Voici la traduction en français :

```markdown
Alternativement :
``````bash
docker-compose pull
docker-compose up --build -d
```

## Utilisation de NGINX

Si vous souhaitez vous débarrasser du :3000 dans l'URL et avoir un domaine personnalisé, vous pouvez utiliser NGINX pour faire un reverse proxy du port 80 vers 3000. Ainsi, l'utilisateur pourra ouvrir l'application en utilisant votre domaine. Exemple : `http://yourdomain.com`.

1. ```bash
   sudo yum install nginx
   ```
2. ```bash
   nginx -v
   ```
3. <pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl start nginx
   </strong></code></pre>
4. <pre class="language-bash"><code class="lang-bash"><strong>sudo nano /etc/nginx/conf.d/flowise.conf
   </strong></code></pre>
5. Copiez-collez ce qui suit et modifiez-le avec votre domaine :

```shell
server {
    listen 80;
    listen [::]:80;
    server_name yourdomain.com; #Example: demo.flowiseai.com
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

press `Ctrl + X` pour quitter, et `Y` pour enregistrer le fichier

6. ```bash
   sudo systemctl restart nginx
   ```
7. Go to your DNS provider, and add a new A record. Name will be your domain name, and value will be the Public IPv4 address from EC2 instance

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt="" width="367"><figcaption></figcaption></figure>

6. You should now be able to open the app: `http://yourdomain.com`.

### Install Certbot to have HTTPS

If you like your app to have `https://yourdomain.com`. Here is how:

1. For installing Certbot and enabling HTTPS on NGINX, we will rely on Python. So, first of all, let's set up a virtual environment:

```bash
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

5. Après avoir suivi l'assistant de génération de certificat, nous pourrons accéder à notre instance EC2 via HTTPS en utilisant l'adresse `https://yourdomain.com`

## Configurer le renouvellement automatique

Pour permettre à Certbot de renouveler automatiquement les certificats, il suffit d'ajouter une tâche cron en exécutant la commande suivante :

```bash
echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

## Félicitations !

Vous avez réussi à configurer les applications Flowise sur une instance EC2 avec un certificat SSL sur votre domaine[🥳](https://emojipedia.org/partying-face/)
