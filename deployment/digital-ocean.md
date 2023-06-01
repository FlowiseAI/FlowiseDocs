# Digital Ocean

## Create Droplet

In this section, we are going to create a Droplet. For more information, refer to [official guide](https://docs.digitalocean.com/products/droplets/quickstart/).

1. First, Click **Droplets** from the dropdown

<figure><img src="../.gitbook/assets/image (15) (2).png" alt=""><figcaption></figcaption></figure>

2. Select Data Region and a Basic $6/mo Droplet type&#x20;

<figure><img src="../.gitbook/assets/image (17) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. Select Authentication Method. In this example, we are going to use Password

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

4. After a while you should be able to see your droplet created successfully

<figure><img src="../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

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
3. Install docket-compose:

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

3. Create a `.env` file and specify PORT. You can use your favourit editor. I'll use `nano`

```bash
nano .env
```

<figure><img src="../.gitbook/assets/image (10).png" alt="" width="375"><figcaption></figcaption></figure>

4. (Optional) You can also specify `FLOWISE_USERNAME` and `FLOWISE_PASSWORD` for app level authorization. See more [setting-username-and-password.md](../authorization/setting-username-and-password.md "mention")
5. Then press `Ctrl + X` to Exit, and `Y` to save the file
6. Run docker compose

```bash
docker-compose up -d
```

7. You can then view the app: "Your Public IPv4 DNS":3000. Example: `176.63.19.226:3000`
8. You can bring the app down by:

```bash
docker-compose stop
```

7. You can pull from latest image by:

```bash
docker pull flowiseai/flowise
```

