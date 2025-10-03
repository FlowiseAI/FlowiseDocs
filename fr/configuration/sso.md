# SSO

{% hint style="info" %}
SSO is only available for Enterprise plan
{% endhint %}

Flowise supports [OIDC](https://openid.net/) that allows users to use _single sign_-on (_SSO_) to access application. Currently only the [Organization Admin](../using-flowise/workspaces.md#setting-up-admin-account) can configure the SSO configurations.

## Microsoft

1. In the Azure portal, search for Microsoft Entra ID:

<figure><img src="../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>

2. From the left hand bar, click App Registrations, then New Registration:

<figure><img src="../.gitbook/assets/image (194).png" alt=""><figcaption></figcaption></figure>

3. Enter an app name, and select Single Tenant:

<figure><img src="../.gitbook/assets/image (195).png" alt=""><figcaption></figcaption></figure>

4. After an app is created, note down the Application (client) ID and Directory (tenant) ID:

<figure><img src="../.gitbook/assets/image (196).png" alt=""><figcaption></figcaption></figure>

5. On the left side bar, click Certificates & secrets -> New client secret -> Add:

<figure><img src="../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

6. After the secret has been created, copy the Value, <mark style="color:red;">not</mark> the Secret ID:

<figure><img src="../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

7. On the left side bar, click Authentication -> Add a platform -> Web:

<figure><img src="../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

8. Fill in the redirect URIs. This will need to be changed depending on how you are hosting it: `http[s]://[your-flowise-instance.com]/api/v1/azure/callback`:

<figure><img src="../.gitbook/assets/image (218).png" alt="" width="514"><figcaption></figcaption></figure>

9. You should be able to see the new Redirect URI created:

<figure><img src="../.gitbook/assets/image (219).png" alt=""><figcaption></figcaption></figure>

10. Back to Flowise app, login as Organization Admin. Navigate to SSO Config from left side bar. Fill in the Azure Tenant ID and Client ID from Step 4, and Client Secret from Step 6. Click Test Configuration to see if the connection can be established successfully:

<figure><img src="../.gitbook/assets/image (220).png" alt="" width="563"><figcaption></figcaption></figure>

11. Lastly, enable and save it:

<figure><img src="../.gitbook/assets/image (221).png" alt="" width="563"><figcaption></figcaption></figure>

12. Before users can sign in using SSO, they have to be invited first. Refer to [Inviting users for SSO sign in](sso.md#inviting-users-for-sso-sign-in) for step by step guide. Invited users must also be part of the Directory Users in Azure.

<figure><img src="../.gitbook/assets/image (2) (1) (2).png" alt=""><figcaption></figcaption></figure>

## Google

To enable Sign In With Google on your website, you first need to set up your Google API client ID. To do so, complete the following steps:

1. Open the **Credentials** page of the [Google APIs console](https://console.developers.google.com/apis).
2. Click **Create credentials** > **OAuth client ID**

<figure><img src="../.gitbook/assets/image (224).png" alt="" width="563"><figcaption></figcaption></figure>

3\. Select **Web Application**:

<figure><img src="../.gitbook/assets/image (225).png" alt="" width="504"><figcaption></figcaption></figure>

4\. Fill in the redirect URIs. This will need to be changed depending on how you are hosting it: `http[s]://[your-flowise-instance.com]/api/v1/google/callback`:

<figure><img src="../.gitbook/assets/image (226).png" alt="" width="563"><figcaption></figcaption></figure>

5\. After creating, grab the client ID and secret:

<figure><img src="../.gitbook/assets/image (227).png" alt=""><figcaption></figcaption></figure>

6\. Back to Flowise app, add the Client ID and secret. Test the connection and Save it.

<figure><img src="../.gitbook/assets/image (228).png" alt="" width="563"><figcaption></figcaption></figure>

## Auth0

1. Register an account on [Auth0](https://auth0.com/), then create a new Application

<figure><img src="../.gitbook/assets/image (229).png" alt=""><figcaption></figcaption></figure>

2. Select **Regular Web Application**:

<figure><img src="../.gitbook/assets/image (230).png" alt=""><figcaption></figcaption></figure>

3. Configure the fields such as Name, Description. Take notes of the **Domain**, **Client ID**, and **Client Secret**.

<figure><img src="../.gitbook/assets/image (231).png" alt=""><figcaption></figcaption></figure>

4\. Fill in the Application URIs. This will need to be changed depending on how you are hosting it: `http[s]://[your-flowise-instance.com]/api/v1/auth0/callback`:

<figure><img src="../.gitbook/assets/image (232).png" alt=""><figcaption></figcaption></figure>

5. In the API’s tab, ensure that Auth0 Management API is enabled with the following permissions
   * read:users
   * read:client\_grants

<figure><img src="../.gitbook/assets/image (233).png" alt=""><figcaption></figcaption></figure>

6\. Back to Flowise App, fill in the Domain, Client ID and Secret. Test and Save the configuration.

<figure><img src="../.gitbook/assets/image (234).png" alt="" width="563"><figcaption></figcaption></figure>

## Inviting users for SSO sign in

In order for new user to be able to login, you have to invite new users into Flowise application. This is essential to keep a record of the role/workspace of the invited user. Refer to [Invite Users](../using-flowise/workspaces.md#invite-user) section for env variables configuration.

Invited user will be receiving invitation link to login:

<figure><img src="../.gitbook/assets/image (222).png" alt="" width="449"><figcaption></figcaption></figure>

Clicking the button will bring the invited user directly to Flowise SSO login screen:

<figure><img src="../.gitbook/assets/image (210).png" alt="" width="400"><figcaption></figcaption></figure>

Or navigate to Flowise app and Sign in with SSO:

<figure><img src="../.gitbook/assets/image (211).png" alt="" width="437"><figcaption></figcaption></figure>
