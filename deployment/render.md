# Render

1. Fork [Flowise Official Repository](https://github.com/FlowiseAI/Flowise)
2. Visit your github profile to assure you have successfully made a fork
3. Sign in to [Render](https://dashboard.render.com)
4. Click "New +"

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

5. Select "Web Service"

![](<../.gitbook/assets/image (9) (1) (1) (1).png>)

6. Connect Your Github Account

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

7. Select your forked Flowise repo and click Connect
8. Fill in your preferred app name, and select Node for Runtime

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

9. Fill in the Build command and Start command as follow:

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

10. Select the plan and Create Web Service.&#x20;

<figure><img src="../.gitbook/assets/image (15) (1).png" alt=""><figcaption></figcaption></figure>

11. If you want to add app level authorization, navigate to Environment tab and add:

* FLOWISE\_USERNAME
* FLOWISE\_PASSWORD

<figure><img src="../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>

Navigate to the deployed URL and that's it [🚀](https://emojipedia.org/rocket/)[🚀](https://emojipedia.org/rocket/)

Detail walkthrough video:

{% embed url="https://youtu.be/s33v5cIeqA4" %}

## Persistent Disk

The default filesystem for services running on Render is ephemeral. Flowise data isn’t persisted across deploys and restarts. To solve this issue, we can use [Render Disk](https://render.com/docs/disks).

1. On the left hand side bar, click **Disks**
2. Name your disk, and specify the **Mount Path** to `/opt/render/.flowise`

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

3. Click the **Environment** section, and add a new environment variable:

* Key: `DATABASE_PATH`
* Value: `/opt/render/.flowise`

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

4. Clear build cache and deploy

<figure><img src="../.gitbook/assets/image (36).png" alt="" width="194"><figcaption></figcaption></figure>

5. Now try creating a flow and save it in Flowise. Then try restarting service or redeploy, you should still be able to see the flow you have saved previously.
