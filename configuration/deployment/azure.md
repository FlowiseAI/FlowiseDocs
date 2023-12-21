# Azure

## Prerequisites

1. _(Optional)_ [Install Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) if you'd like to follow the cli based commands

## Create a Container Instance without Persistent Storage

Without persistent storage your data is kept in memory. This means that on a container restart, all the data that you stored will disappear.

### In Portal

1. Search for Container Instances in Marketplace and click Create:

<figure><img src="../../.gitbook/assets/azure/deployment/1.png" alt=""><figcaption><p>Container Instances entry in Azure's Marketplace</p></figcaption></figure>

2. Select or create a Resource group, Container name, Region, Image source `Other registry`, Image type, Image `flowiseai/flowise`, OS type and Size. Then click "Next: Networking" to configure Flowise ports:

<figure><img src="../../.gitbook/assets/azure/deployment/2.png" alt=""><figcaption><p>First page in the Container Instance create wizard</p></figcaption></figure>

3. Add a new port `3000 (TCP)` next to the default `80 (TCP)`. Then Select "Next: Advanced":

<figure><img src="../../.gitbook/assets/azure/deployment/3.png" alt=""><figcaption><p>Second page in the Container Instance create wizard. It asks for netowrking type and ports.</p></figcaption></figure>

4. Set Restart policy to `On failure`. Next, add 2 Environment variables `FLOWISE_USERNAME` and `FLOWISE_PASSWORD`. Add Command override `["/bin/sh", "-c", "flowise start"]`. Finally click "Review + create":

<figure><img src="../../.gitbook/assets/azure/deployment/4.png" alt=""><figcaption><p>Third page in the Container Instance create wizard. It asks for restart policy, environment variables and command that runs on container start.</p></figcaption></figure>

5. Review final settings and click "Create":

<figure><img src="../../.gitbook/assets/azure/deployment/5.png" alt=""><figcaption><p>Final review and create page for a Container Instance.</p></figcaption></figure>

6. Once creation is completed, click on "Go to resource"

<figure><img src="../../.gitbook/assets/azure/deployment/6.png" alt=""><figcaption><p>Resource creation result page in Azure.</p></figcaption></figure>

7. Visit your Flowise instance by copying IP address and adding :3000 as a port:

<figure><img src="../../.gitbook/assets/azure/deployment/7.png" alt=""><figcaption><p>Container Instance overview page</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/azure/deployment/8.png" alt=""><figcaption><p>Flowise application deployed as Container Instance</p></figcaption></figure>

### Create using Azure CLI

1. Create a resource group (if you don't already have one)

```bash
az group create --name flowise-rg --location "West US"
```

2. Create a Container Instance

```bash
az container create -g flowise-rg \
	--name flowise \
	--image flowiseai/flowise \
	--command-line "/bin/sh -c 'flowise start'" \
	--environment-variables FLOWISE_USERNAME=flowise-user FLOWISE_PASSWORD=flowise-password \
	--ip-address public \
	--ports 80 3000 \
	--restart-policy OnFailure
```

3. Visit the IP address (including port :3000) printed from the output of the above command.

## Create a Container Instance with Persistent Storage

The creation of a Container Instance with persistent storage is only possible using CLI:

1. Create a resource group (if you don't already have one)

```bash
az group create --name flowise-rg --location "West US"
```

2. Create the Storage Account resource (or use existing one) inside above resource group. You can check how to do it [here](https://learn.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal).
3. Inside Azure Storage create new File share. You can check how to do it [here](https://learn.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal).
4. Create a Container Instance

```bash
az container create -g flowise-rg \
	--name flowise \
	--image flowiseai/flowise \
	--command-line "/bin/sh -c 'flowise start'" \
	--environment-variables FLOWISE_USERNAME=flowise-user FLOWISE_PASSWORD=flowise-password DATABASE_PATH=/opt/flowise/.flowise APIKEY_PATH=/opt/flowise/.flowise SECRETKEY_PATH=/opt/flowise/.flowise LOG_PATH=/opt/flowise/.flowise/logs \
	--ip-address public \
	--ports 80 3000 \
	--restart-policy OnFailure \
	--azure-file-volume-share-name here goes the name of your File share \
	--azure-file-volume-account-name here goes the name of your Storage Account \
	--azure-file-volume-account-key here goes the access key to your Storage Account \
	--azure-file-volume-mount-path /opt/flowise/.flowise
```

5. Visit the IP address (including port :3000) printed from the output of the above command.
6. From now on your data will be stored in an SQLite database which you can find in your File share.

Watch video tutorial on deploying to Azure Container Instance:

{% embed url="https://www.youtube.com/watch?v=yDebxDfn2yk" %}
