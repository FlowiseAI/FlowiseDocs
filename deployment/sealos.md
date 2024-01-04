# Sealos

1. Click the following prebuilt [template](https://cloud.sealos.io/?openapp=system-template%3FtemplateName%3Dflowise)
2. add authorization

* FLOWISE\_USERNAME
* FLOWISE\_PASSWORD

<figure><img src="../.gitbook/assets/sealos/deployment/1.jpg" alt=""><figcaption><p>Sealos's template for flowise </p></figcaption></figure>

3. click "Deploy Application" on the template page to start deployment.
4. Once deployment concludes, click "Details" to navigate to the application's details.

<figure><img src="../.gitbook/assets/sealos/deployment/2.png" alt=""><figcaption><p>Detail page for flowise application </p></figcaption></figure>

5. Wait for the application's status to switch to running. Subsequently, click on the external link to open the application's Web interface directly through the external domain.

<figure><img src="../.gitbook/assets/sealos/deployment/3.png" alt=""><figcaption><p>external link for flowise application </p></figcaption></figure>

## Persistent Volume

just click "Update" top-right on the app details page, then click "Advanced" -> "Add volume", Fill in the value of "mount path": `/root/.flowise`.

<figure><img src="../.gitbook/assets/sealos/deployment/4.png" alt=""><figcaption><p>Persistent Volume for flowise application </p></figcaption></figure>

To wrap up, click the "Deploy" button.

Now try creating a flow and save it in Flowise. Then try restarting service or redeploy, you should still be able to see the flow you have saved previously.