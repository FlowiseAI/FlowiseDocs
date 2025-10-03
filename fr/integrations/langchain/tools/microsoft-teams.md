# Microsoft Teams

## Pre-requisite

A valid Microsoft 365 license assigned to the Azure Active Directory user. Refer: [https://learn.microsoft.com/en-us/answers/questions/761931/microsoft-graph-api-throws-the-mailbox-is-either-i](https://learn.microsoft.com/en-us/answers/questions/761931/microsoft-graph-api-throws-the-mailbox-is-either-i)

## Create credential in Flowise

1. Add a new Microsoft Teams OAuth2 credential
2. Enter a name for the credential.
3. Copy the OAuth Redirect URL.
4. Note that the following fields need to be filled in:
   * Authorization URL
   * Access Token URL
   * Client ID
   * Client Secret

<figure><img src="../../../.gitbook/assets/image (209).png" alt="" width="436"><figcaption></figcaption></figure>

## Create an Azure application <a href="#h_8276f6aa94" id="h_8276f6aa94"></a>

1. Login to your existing [Azure](https://login.microsoftonline.com/) account or [sign up](https://signup.live.com/signup) if you haven't already signed up
2. Search for **App registrations**.
3. Next, register a new Azure application in [app registrations](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/CreateApplicationBlade/quickStartType~/null/isMSAApp~/false).

<figure><img src="../../../.gitbook/assets/image (192).png" alt="" width="304"><figcaption></figcaption></figure>

4. Under "Redirect URI (optional)", select "Web" and paste your "OAuth Redirect URL" you copied earlier.

<figure><img src="../../../.gitbook/assets/image (197).png" alt=""><figcaption></figcaption></figure>

5. After created application, navigate to **Certificates & secrets** > **Client secrets** and click on the "**New client secret**" button to create a client secret. Copy the secret to use later.

<figure><img src="../../../.gitbook/assets/image (200).png" alt=""><figcaption></figcaption></figure>

6. Navigate to **Overview** and click "**Endpoints**". Copy the endpoints for "**OAuth 2.0 authorization endpoint (v2)**" and "**OAuth 2.0 token endpoint (v2)**".

<figure><img src="../../../.gitbook/assets/image (202).png" alt=""><figcaption></figcaption></figure>

7. Close the Endpoints popup, copy the **Application (client) ID**:

<figure><img src="../../../.gitbook/assets/image (203).png" alt="" width="563"><figcaption></figcaption></figure>

## Finish setup in Flowise

1. Fill in all the values copied earlier. Then click "**Authenticate**":

<figure><img src="../../../.gitbook/assets/image (212).png" alt="" width="437"><figcaption></figcaption></figure>

2. A Microsoft window will pop up, select the account.

{% hint style="warning" %}
The account user must have a valid Microsoft 365 licensed
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (205).png" alt="" width="459"><figcaption></figcaption></figure>

3. Grant the required permissions:

<figure><img src="../../../.gitbook/assets/image (237).png" alt="" width="445"><figcaption></figcaption></figure>

4. The pop up will automatically close, and credential will be saved afterwards.

## Use as Agent Tool

Multiple actions can be selected to let the Agent intelligently choose the appropriate one.\
Parameters can be left empty to allow the Agent to determine the values on its own. However, if the user provides values, those will override the Agent's choices.

<figure><img src="../../../.gitbook/assets/image (217).png" alt=""><figcaption></figcaption></figure>

## Use as Tool Node

It can also be used as a Tool Node in a determined workflow scenario. For example, retrieving a list of Teams messages before proceeding to the next step.\
In this mode, **Tool Input Arguments must be explicitly defined and filled with values**.\
Unlike the [**Use as Agent Tool**](microsoft-teams.md#use-as-agent-tool) option, there is no Agent to automatically determine the inputs. The user must manually populate the fields, either by entering fixed values or using variables enclosed in double curly brackets `{{ }}`.

<figure><img src="../../../.gitbook/assets/image (223).png" alt=""><figcaption></figcaption></figure>
