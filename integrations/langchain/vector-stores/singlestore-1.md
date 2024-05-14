# SingleStore

## Setup

1. Register an account on [SingleStore](https://www.singlestore.com/)
2. Login to portal. On the left side panel, click **CLOUD** -> **Create new workspace group.** Then click **Create Workspace** button.

<figure><img src="../../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

3. Select cloud provider and data region, then click **Next**:

<figure><img src="../../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

4. Review and click **Create Workspace**:

<figure><img src="../../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

5. You should now see your workspace created:

<figure><img src="../../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

6. Proceed to create a database

<figure><img src="../../../.gitbook/assets/image (65).png" alt="" width="485"><figcaption></figcaption></figure>

7. You should be able to see your database created and attached to the workspace:

<figure><img src="../../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

8. Click Connect from the workspace dropdown -> Connect Directly:

<figure><img src="../../../.gitbook/assets/image (61).png" alt="" width="418"><figcaption></figcaption></figure>

9. You can specify a new password or use the default generated one. Then click Continue:

<figure><img src="../../../.gitbook/assets/image (62).png" alt="" width="485"><figcaption></figcaption></figure>

10. On the tabs, switch to **Your App**, and select **Node.js** from the dropdown. Take note/save the `Username`, `Host`, `Password` as you will need these in Flowise later.

<figure><img src="../../../.gitbook/assets/image (63).png" alt="" width="563"><figcaption></figcaption></figure>

11. Back to Flowise canvas, drag and drop SingleStore nodes. Click **Create New** from the Credentials dropdown:

<figure><img src="../../../.gitbook/assets/image (4) (1) (2) (1).png" alt="" width="271"><figcaption></figcaption></figure>

11. Put in the Username and Password from above:

<figure><img src="../../../.gitbook/assets/image (64).png" alt="" width="563"><figcaption></figcaption></figure>

13. Then specify the Host and Database Name:

<figure><img src="../../../.gitbook/assets/image (5) (1) (2).png" alt="" width="272"><figcaption></figcaption></figure>

13. Now you can start upserting data with SingleStore:

<figure><img src="../../../.gitbook/assets/image (6) (1) (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (7) (1) (2).png" alt=""><figcaption></figcaption></figure>

14. Navigate back to SingleStore portal, and to your database, you will be able to see all the data that has been upserted:

<figure><img src="../../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>
