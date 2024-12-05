---
description: Learn how to deploy Flowise on Railway
---

# Railway

***

1. Click the following prebuilt [template](https://railway.app/template/pn4G8S?referralCode=WVNPD9)
2. Click Deploy Now

<figure><img src="../../.gitbook/assets/image (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

3. Change to your preferred repository name and click Deploy

<figure><img src="../../.gitbook/assets/image (2) (1) (2) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. If succeeds, you should be able to see a deployed URL

<figure><img src="../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

5. To add authorization, navigate to Variables tab and add:

* FLOWISE\_USERNAME
* FLOWISE\_PASSWORD

<figure><img src="../../.gitbook/assets/image (15) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. There are list of env variables you can configure. Refer to [environment-variables.md](../environment-variables.md "mention")

That's it! You now have a deployed Flowise on Railway [🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)

## Persistent Volume

The default filesystem for services running on Railway is ephemeral. Flowise data isn’t persisted across deploys and restarts. To solve this issue, we can use [Railway Volume](https://docs.railway.app/reference/volumes).

To ease the steps, we have a Railway template with volume mounted: [https://railway.app/template/nEGbjR](https://railway.app/template/nEGbjR)

Just click Deploy and fill in the Env Variables like below:

* DATABASE\_PATH - `/opt/railway/.flowise`
* APIKEY\_PATH - `/opt/railway/.flowise`
* LOG\_PATH - `/opt/railway/.flowise/logs`
* SECRETKEY\_PATH - `/opt/railway/.flowise`
* BLOB\_STORAGE\_PATH - `/opt/railway/.flowise/storage`

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="420"><figcaption></figcaption></figure>

Now try creating a flow and save it in Flowise. Then try restarting service or redeploy, you should still be able to see the flow you have saved previously.
