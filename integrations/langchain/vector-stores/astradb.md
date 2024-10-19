# AstraDB

## Setup

1. Register an account on [AstraDB](https://astra.datastax.com/)
2. Login to portal. Create a Database

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

3. Choose Serverless (Vector), fill in the Database name, Provider, and Region

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

4. After database has been setup, grab the API Endpoint, and generate Application Token

<figure><img src="../../../.gitbook/assets/Picture7.png" alt=""><figcaption></figcaption></figure>

5. Create a new collection, select the desired dimenstion and similarity metric:

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

6. Back to Flowise canvas, drag and drop Astra node. Click **Create New** from the Credentials dropdown:

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (2).png" alt="" width="235"><figcaption></figcaption></figure>

7. Specify the API Endpoint and Application Token:

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

8. You can now upsert data to AstraDB

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

9. Navigate back to Astra portal, and to your collection, you will be able to see all the data that has been upserted:

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

10. Start querying!

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
