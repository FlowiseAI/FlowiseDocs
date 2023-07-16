# Azure

## Prerequisites

1. \[Optional] [Install Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) if you'd like to follow the cli based commands

## Create a Container Instance without Persistent Storage

### In Portal

1. Search for Container Instances in Marketplace and click Create:

<figure><img src="../.gitbook/assets/1.png" alt=""><figcaption><p>Container Instances entry in Azure's Marketplace</p></figcaption></figure>

2. Select or create a Resource group, Container name, Region, Image source `Other registry`, Image type, Image `flowiseai/flowise` OS type and Size. Then click "Next: Networking" to configure Flowise ports:

<figure><img src="../.gitbook/assets/2.png" alt=""><figcaption><p>First page in the Container Instance create wizard</p></figcaption></figure>

3. Add a new port `3000 (TCP)` next to the default `80 (TCP)`. Then Select "Next: Advanced":

<figure><img src="../.gitbook/assets/3.png" alt=""><figcaption><p>Second page in the Container Instance create wizard. It asks for netowrking type and ports.</p></figcaption></figure>

4. Set Restart policy to `On failure`. Next, add 2 Environment variables `FLOWISE_USER` and `FLOWISE_PASSWORD`. Add Command override. Finally click "Review + create":

<figure><img src="../.gitbook/assets/4.png" alt=""><figcaption><p>Third page in the Container Instance create wizard. It asks for restart policy, environment variables and command that runs on container start.</p></figcaption></figure>

5. Review final settings and click "Create":

<figure><img src="../.gitbook/assets/5.png" alt=""><figcaption><p>Final review and create page for a Container Instance.</p></figcaption></figure>

6. Once creation is completed, click on "Go to resource"

<figure><img src="../.gitbook/assets/6.png" alt=""><figcaption><p>Resource creation result page in Azure.</p></figcaption></figure>

7. Visit your Flowise instance by copying IP address and adding :3000 as a port:

<figure><img src="../.gitbook/assets/7.png" alt=""><figcaption><p>Container Instance overview page</p></figcaption></figure>

<figure><img src="../.gitbook/assets/8.png" alt=""><figcaption><p>Flowise application deployed as Container Instance</p></figcaption></figure>

### Create using Azure CLI

1. Create a resource group (if you don't already have one)

```bash
az group create --name flowise-rg --location "West US"
```

2. Create a Container App Environment

```bash
az containerapp env create --name flowise-env --resource-group flowise-rg --location "West US"
```

3. Create a Container App

```bash
az containerapp create \
  --name flowise \
  --resource-group flowise-rg \
  --environment flowise-env  \
  --image docker.io/flowiseai/flowise:latest \
  --ingress external \
  --target-port 3000 \
  --command "flowise" "start" \
  --secrets password="p@ssw0ord!" \
  --env-vars FLOWISE_USERNAME="flowise-user" FLOWISE_PASSWORD="secretref:password"
  --query properties.configuration.ingress.fqdn
```

4. Visit the url printed from the output of the previous command.
