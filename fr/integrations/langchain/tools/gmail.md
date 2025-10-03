# Gmail

## Create credential in Flowise

1. Add a new Gmail OAuth2 credential
2. Enter a name for the credential.
3. Copy the OAuth Redirect URL.
4. Note that the following fields need to be filled in:
   * Client ID
   * Client Secret

<figure><img src="../../../.gitbook/assets/image (255).png" alt="" width="437"><figcaption></figcaption></figure>

## Create/Use Google Project

1. Log in to your [**Google Cloud**](https://console.cloud.google.com/) account.
2. Navigate to [**Google Cloud Console > APIs & Services**](https://console.cloud.google.com/apis/credentials), and select the project you want to use from the dropdown at the top left (or create a new project and select it).
3. Set up the **OAuth consent screen** if you haven't configured one before.

<figure><img src="../../../.gitbook/assets/image (256).png" alt="" width="563"><figcaption></figcaption></figure>

4. Go to **Credentials**, then click **+ CREATE CREDENTIALS > OAuth client ID**.

<figure><img src="../../../.gitbook/assets/image (257).png" alt="" width="563"><figcaption></figcaption></figure>

5. In the **Application type** dropdown, select **Web application**.
6. Under **Authorized redirect URIs**, click **+ ADD URI** and paste the OAuth redirect URL copied earlier.
7. Click **Create**.

<figure><img src="../../../.gitbook/assets/image (258).png" alt="" width="407"><figcaption></figcaption></figure>

8. Copy the Client ID and Client Secret:

<figure><img src="../../../.gitbook/assets/image (259).png" alt="" width="489"><figcaption></figcaption></figure>

9. In **Enabled APIs & Services**, click **+ ENABLE APIS AND SERVICES**.
10. Search for and enable the **Gmail API**.

<figure><img src="../../../.gitbook/assets/image (260).png" alt="" width="538"><figcaption></figcaption></figure>

11. Return to **Credentials**, click the newly created credential under **OAuth 2.0 Client IDs**, and on the detail page, you’ll find the **Client ID** and **Client Secret**.

## Finish setup in Flowise

1. Fill in all the values copied earlier. Then click "**Authenticate**":

<figure><img src="../../../.gitbook/assets/image (262).png" alt="" width="433"><figcaption></figcaption></figure>

2. A Google login window will pop up:

<figure><img src="../../../.gitbook/assets/image (261).png" alt="" width="448"><figcaption></figcaption></figure>

3. Grant the permissions:

<figure><img src="../../../.gitbook/assets/image (263).png" alt="" width="373"><figcaption></figcaption></figure>

4. Pop up window will be closed automatically and credential will be saved and ready to be used.

## Use as Agent Tool

Multiple actions can be selected to let the Agent intelligently choose the appropriate one.\
Parameters can be left empty to allow the Agent to determine the values on its own. However, if the user provides values, those will override the Agent's choices.

<figure><img src="../../../.gitbook/assets/image (264).png" alt=""><figcaption></figcaption></figure>

## Use as Tool Node

It can also be used as a Tool Node in a determined workflow scenario. For example, retrieving a list of draft messages before proceeding to the next step.\
In this mode, **Tool Input Arguments must be explicitly defined and filled with values**.\
Unlike the [**Use as Agent Tool**](gmail.md#use-as-agent-tool) option, there is no Agent to automatically determine the inputs. The user must manually populate the fields, either by entering fixed values or using variables enclosed in double curly brackets `{{ }}`.

<figure><img src="../../../.gitbook/assets/image (265).png" alt=""><figcaption></figcaption></figure>
