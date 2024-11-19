---
description: Learn how to deploy Flowise on AWS
---

# AWS

***

## Prerequisite

This requires some basic understanding of how AWS works.

Two options are available to deploy Flowise on AWS:

* [Deploy on ECS using CloudFormation](aws.md#deploy-on-ecs-using-cloudformation)
* [Manually configure an EC2 Instance](aws.md#launch-ec2-instance)

## Deploy on ECS using CloudFormation

CloudFormation template is available here: [https://gist.github.com/MrHertal/549b31a18e350b69c7200ae8d26ed691](https://gist.github.com/MrHertal/549b31a18e350b69c7200ae8d26ed691)

It deploys Flowise on an ECS cluster exposed through ELB.

It was inspired by this reference architecture: [https://github.com/aws-samples/ecs-refarch-cloudformation](https://github.com/aws-samples/ecs-refarch-cloudformation)

Feel free to edit this template to adapt things like Flowise image version, environment variables etc.

Example of command to deploy Flowise using the [AWS CLI](https://aws.amazon.com/fr/cli/):

```bash
aws cloudformation create-stack --stack-name flowise --template-body file://flowise-cloudformation.yml --capabilities CAPABILITY_IAM
```

After deployment, the URL of your Flowise application is available in the CloudFormation stack outputs.

## Deploy on ECS using Terraform

The Terraform files (`variables.tf`, `main.tf`) are available in this GitHub repository: [terraform-flowise-setup](https://github.com/huiseo/terraform-flowise-setup/tree/main).

This setup deploys Flowise on an ECS cluster exposed through an Application Load Balancer (ALB). It is based on AWS best practices for ECS deployments.

You can modify the Terraform template to adjust:

* Flowise image version
* Environment variables
* Resource configurations (CPU, memory, etc.)

### Example Commands for Deployment:

1. **Initialize Terraform:**

```bash
terraform init
terraform apply
terraform destroy
```

## Launch EC2 Instance

1. In the EC2 dashboard, click **Launch Instance**

<figure><img src="../../.gitbook/assets/image (19) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Scroll down and **Create new key pair** if you don't have one

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

3. Fill in your preferred key pair name. For Windows, we will use `.ppk` and PuTTY to connect to the instance. For Mac and Linux, we will use `.pem` and OpenSSH

<figure><img src="../../.gitbook/assets/image (15) (2) (1).png" alt="" width="370"><figcaption></figcaption></figure>

4. Click **Create key pair** and select a location path to save the `.ppk` file
5. Open the left side bar, and open a new tab from **Security Groups**. Then **Create security group**

<figure><img src="../../.gitbook/assets/image (20) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. Fill in your preferred security group name and description. Next, add the following to Inbound Rules and **Create security group**

<figure><img src="../../.gitbook/assets/image (12) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

7. Back to the first tab (EC2 Launch an instance) and scroll down to **Network settings**. Select the security group you've just created

<figure><img src="../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

8. Click **Launch instance**. Navigate back to EC2 Dashboard, after few mins we should be able to see a new instance up and running [🎉](https://emojipedia.org/party-popper/)

<figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## How to Connect to your instance (Windows)

1. For Windows, we are going to use PuTTY. You can download one from [here](https://www.chiark.greenend.org.uk/\~sgtatham/putty/latest.html).
2. Open PuTTY and fill in the **HostName** with your instance's Public IPv4 DNS name

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. From the left hand side bar of PuTTY Configuration, expand **SSH** and click on **Auth**. Click Browse and select the `.ppk` file you downloaded earlier.

<figure><img src="../../.gitbook/assets/image (23) (1) (1).png" alt="" width="296"><figcaption></figcaption></figure>

4. Click **Open** and **Accept** the pop up message

<figure><img src="../../.gitbook/assets/image (18) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

5. Then login as `ec2-user`

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

6. Now you are connected to the EC2 instance

## How to Connect to your instance (Mac and Linux)

1. Open the Terminal application on your Mac/Linux.
2. _(Optional)_ Set the permissions of the private key file to restrict access to it:

```bash
chmod 400 /path/to/mykey.pem
```

3. Use the `ssh` command to connect to your EC2 instance, specifying the username (`ec2-user`), Public IPv4 DNS, and the path to the `.pem` file.

```bash
ssh -i /Users/username/Documents/mykey.pem ec2-user@ec2-123-45-678-910.compute-1.amazonaws.com
```

4. Press Enter, and if everything is configured correctly, you should successfully establish an SSH connection to your EC2 instance

## Install Docker

1. Apply pending updates using the yum command:

```bash
sudo yum update
```

2. Search for Docker package:

```bash
sudo yum search docker
```

3. Get version information:

```bash
sudo yum info docker
```

4. Install docker, run:

```bash
sudo yum install docker
```

5. Add group membership for the default ec2-user so you can run all docker commands without using the sudo command:

```bash
sudo usermod -a -G docker ec2-user
id ec2-user
newgrp docker
```

6. Install docker-compose:

```bash
sudo yum install docker-compose-plugin
```

7. Enable docker service at AMI boot time:

```bash
sudo systemctl enable docker.service
```

8. Start the Docker service:

```bash
sudo systemctl start docker.service
```

## Install Git

```bash
sudo yum install git -y
```

## Setup

1. Clone the repo

```bash
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

<figure><img src="../../.gitbook/assets/image (13) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. Specify the env variables:

```sh
PORT=3000
DATABASE_PATH=/root/.flowise
APIKEY_PATH=/root/.flowise
SECRETKEY_PATH=/root/.flowise
LOG_PATH=/root/.flowise/logs
BLOB_STORAGE_PATH=/root/.flowise/storage
```

5. _(Optional)_ You can also specify `FLOWISE_USERNAME` and `FLOWISE_PASSWORD` for app level authorization. See more [broken-reference](broken-reference/ "mention")
6. Then press `Ctrl + X` to Exit, and `Y` to save the file
7. Run docker compose

```bash
docker compose up -d
```

7. Your application is now ready at your Public IPv4 DNS on port 3000:

```
http://ec2-123-456-789.compute-1.amazonaws.com:3000
```

8. You can bring the app down by:

```bash
docker compose stop
```

9. You can pull from latest image by:

```bash
docker pull flowiseai/flowise
```

Alternatively:

```bash
docker-compose pull
docker-compose up --build -d
```

## Using NGINX

If you want to get rid of the :3000 on the url and have a custom domain, you can use NGINX to reverse proxy port 80 to 3000 So user will be able to open the app using your domain. Example: `http://yourdomain.com`.

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
5. Copy paste the following and change to your domain:

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

press `Ctrl + X` to Exit, and `Y` to save the file

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

5. After following the certificate generation wizard, we will be able to access our EC2 instance via HTTPS using the address `https://yourdomain.com`

## Set up automatic renewal

To enable Certbot to automatically renew the certificates, it is sufficient to add a cron job by running the following command:

```bash
echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

## Congratulations!

You have successfully setup Flowise apps on EC2 instance with SSL certificate on your domain[🥳](https://emojipedia.org/partying-face/)
