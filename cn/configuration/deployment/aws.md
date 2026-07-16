---
description: 了解如何在 AWS 上部署 Flowise
---

# AWS

***

## 先决条件

这需要对 AWS 的工作原理有一些基本的了解。

有两个选项可用于在 AWS 上部署 Flowise：

* [使用 CloudFormation 在 ECS 上部署](aws.md#deploy-on-ecs-using-cloudformation)
* [手动配置 EC2 实例](aws.md#launch-ec2-instance)

## 使用 CloudFormation 在 ECS 上部署

CloudFormation 模板可在此处获取：[https://gist.github.com/MrHertal/549b31a18e350b69c7200ae8d26ed691](https://gist.github.com/MrHertal/549b31a18e350b69c7200ae8d26ed691)

它将 Flowise 部署在通过 ELB 公开的 ECS 集群上。

它的灵感来自于这个参考架构：[https://github.com/aws-samples/ecs-refarch-cloudformation](https://github.com/aws-samples/ecs-refarch-cloudformation)

请随意编辑此模板以适应 Flowise 图像版本、环境变量等内容。

使用 [AWS CLI](https://aws.amazon.com/fr/cli/) 部署 Flowise 的命令示例：

```bash
aws cloudformation create-stack --stack-name flowise --template-body file://flowise-cloudformation.yml --capabilities CAPABILITY_IAM
```

部署后，您的 Flowise 应用程序的 URL 将在 CloudFormation 堆栈输出中可用。

## 使用 Terraform 在 ECS 上部署

Terraform 文件（`variables.tf`、`main.tf`）可在此 GitHub 存储库中获取：[terraform-flowise-setup](https://github.com/huiseo/terraform-flowise-setup/tree/main)。

此设置将 Flowise 部署在通过应用程序负载均衡器 (ALB) 公开的 ECS 集群上。它基于 ECS 部署的 AWS 最佳实践。

您可以修改 Terraform 模板进行调整：

* Flowise图像版本
* 环境变量
* 资源配置（CPU、内存等）

### 部署命令示例：

1. **初始化Terraform：**

```bash
terraform init
terraform apply
terraform destroy
```

## 启动 EC2 实例

1. 在 EC2 仪表板中，单击 **启动实例**

<figure><img src="../../.gitbook/assets/image (19) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. 向下滚动并**创建新的密钥对**（如果您没有）

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

3. 填写您首选的密钥对名称。对于 Windows，我们将使用 `.ppk` 和 PuTTY 连接到实例。对于 Mac 和 Linux，我们将使用 `.pem` 和 OpenSSH

<figure><img src="../../.gitbook/assets/image (15) (2) (1).png" alt="" width="370"><figcaption></figcaption></figure>

4、点击**创建密钥对**，选择保存`.ppk`文件的位置路径
5. 打开左侧栏，然后从 **安全组** 打开一个新选项卡。然后**创建安全组**

<figure><img src="../../.gitbook/assets/image (20) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. 填写您首选的安全组名称和描述。接下来，将以下内容添加到入站规则并**创建安全组**

<figure><img src="../../.gitbook/assets/image (12) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

7. 返回第一个选项卡（EC2 启动实例）并向下滚动到 **网络设置**。选择刚刚创建的安全组

<figure><img src="../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

8. 单击**启动实例**。导航回 EC2 仪表板，几分钟后我们应该能够看到一个新实例已启动并正在运行 [🎉](https://emojipedia.org/party-popper/)

<figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 如何连接到您的实例 (Windows)

1. 对于 Windows，我们将使用 PuTTY。您可以从[此处](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)下载一个。
2. 打开 PuTTY 并使用实例的公共 IPv4 DNS 名称填写 **HostName**

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. 在 PuTTY 配置的左侧栏中，展开 **SSH** 并单击 **Auth**。单击浏览并选择您之前下载的 `.ppk` 文件。

<figure><img src="../../.gitbook/assets/image (23) (1) (1).png" alt="" width="296"><figcaption></figcaption></figure>

4. 单击“**打开**”和“**接受**”弹出消息

<figure><img src="../../.gitbook/assets/image (18) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

5. 然后以 `ec2-user` 身份登录

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

6. 现在您已连接到 EC2 实例

## 如何连接到您的实例（Mac 和 Linux）

1. 在 Mac/Linux 上打开终端应用程序。
2._（可选）_ 设置私钥文件的权限以限制对其的访问：

```bash
chmod 400 /path/to/mykey.pem
```

3. 使用 `ssh` 命令连接到 EC2 实例，指定用户名 (`ec2-user`)、公共 IPv4 DNS 以及 `.pem` 文件的路径。

```bash
ssh -i /Users/username/Documents/mykey.pem ec2-user@ec2-123-45-678-910.compute-1.amazonaws.com
```

4. 按 Enter，如果所有配置均正确，您应该成功建立与 EC2 实例的 SSH 连接

## 安装 Docker

1. 使用 yum 命令应用挂起的更新：

```bash
sudo yum update
```

2.搜索Docker包：

```bash
sudo yum search docker
```

3.获取版本信息：

```bash
sudo yum info docker
```

4.安装docker，运行：

```bash
sudo yum install docker
```

5. 为默认 ec2-user 添加组成员身份，以便您无需使用 sudo 命令即可运行所有 docker 命令：

```bash
sudo usermod -a -G docker ec2-user
id ec2-user
newgrp docker
```

6.安装docker-compose：

```bash
sudo yum install docker-compose-plugin
```

7. 在 AMI 启动时启用 docker 服务：

```bash
sudo systemctl enable docker.service
```

8.启动Docker服务：

```bash
sudo systemctl start docker.service
```

## 安装 Git

```bash
sudo yum install git -y
```

## 设置

1. 克隆仓库

```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2. cd 进入 docker 文件夹

```bash
cd Flowise && cd docker
```

3. 创建 `.env` 文件。您可以使用您最喜欢的编辑器。我将使用 `nano`

```bash
nano .env
```

<figure><img src="../../.gitbook/assets/image (13) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. 指定环境变量：

```sh
PORT=3000
DATABASE_PATH=/root/.flowise
SECRETKEY_PATH=/root/.flowise
LOG_PATH=/root/.flowise/logs
BLOB_STORAGE_PATH=/root/.flowise/storage
```

5. 然后按 `Ctrl + X` 退出，按 `Y` 保存文件
6.运行docker compose

```bash
docker compose up -d
```

7. 您的应用程序现已在端口 3000 上的公共 IPv4 DNS 上准备就绪：

```
http://ec2-123-456-789.compute-1.amazonaws.com:3000
```

8. 您可以通过以下方式关闭该应用程序：

```bash
docker compose stop
```

9. 您可以通过以下方式提取最新图像：

```bash
docker pull flowiseai/flowise
```

或者：

```bash
docker-compose pull
docker-compose up --build -d
```

## 使用 NGINX

如果您想摆脱网址上的 :3000 并拥有自定义域，您可以使用 NGINX 将代理端口 80 反向到 3000，这样用户将能够使用您的域打开应用程序。示例：`http://yourdomain.com`。

1. ```bash
   sudo yum 安装 nginx
   ```
2. ```bash
   nginx -v
   ```
3.<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl start nginx
   </strong></code></pre>
4.<pre class="language-bash"><code class="lang-bash"><strong>sudo nano /etc/nginx/conf.d/flowise.conf
   </strong></code></pre>
5. 复制粘贴以下内容并更改为您的域：

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

按 `Ctrl + X` 退出，按 `Y` 保存文件

6.```bash
   sudo systemctl 重新启动 nginx
   ```
7. Go to your DNS provider, and add a new A record. Name will be your domain name, and value will be the Public IPv4 address from EC2 instance

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt="" width="367"><figcaption></figcaption></figure>

6. You should now be able to open the app: `http://yourdomain.com`.

### Install Certbot to have HTTPS

If you like your app to have `https://yourdomain.com`. Here is how:

1. For installing Certbot and enabling HTTPS on NGINX, we will rely on Python. So, first of all, let's set up a virtual environment:

```bash
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --升级 pip
```

2. Afterwards, run this command to install Certbot:

```bash
sudo /opt/certbot/bin/pip 安装 certbot certbot-nginx
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
echo "0 0,12 * * * root /opt/certbot/bin/python -c '导入随机;导入时间;time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

## Congratulations!

You have successfully setup Flowise apps on EC2 instance with SSL certificate on your domain[🥳](https://emojipedia.org/partying-face/)
