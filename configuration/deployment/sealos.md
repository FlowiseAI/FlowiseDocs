---
description: Learn how to deploy Flowise on Sealos
---

# Sealos

***

1. Click the following prebuilt [template](https://template.cloud.sealos.io/deploy?templateName=flowise)
2. Add authorization
   * FLOWISE\_USERNAME
   * FLOWISE\_PASSWORD

<figure><img src="../../.gitbook/assets/1.jpg" alt=""><figcaption></figcaption></figure>

3. Click "Deploy Application" on the template page to start deployment.
4. Once deployment concludes, click "Details" to navigate to the application's details.

<figure><img src="../../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

5. Wait for the application's status to switch to running. Subsequently, click on the external link to open the application's Web interface directly through the external domain.

<figure><img src="../../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

## Persistent Volume

Click "Update" top-right on the app details page, then click "Advanced" -> "Add volume", Fill in the value of "mount path": `/root/.flowise`.

<figure><img src="../../.gitbook/assets/4.png" alt="" width="375"><figcaption></figcaption></figure>

To wrap up, click the "Deploy" button.

Now try creating a flow and save it in Flowise. Then try restarting service or redeploy, you should still be able to see the flow you have saved previously.
