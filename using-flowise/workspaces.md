# Workspaces

{% hint style="info" %}
Workspaces is only available for Enterprise for now. Coming soon to Cloud Pro plan
{% endhint %}

Upon your initial login, a default workspace will be automatically generated for you. Workspaces serve to partition resources among various teams or business units. Inside each workspace, Role-Based Access Control (RBAC) is used to manage permissions and access, ensuring users have access only to the resources and settings required for their role.

<figure><img src="../.gitbook/assets/Untitled-2024-10-19-0050.png" alt=""><figcaption></figcaption></figure>

## Setting up Admin Account

<details>

<summary>For self-hosted enterprise, following env variables must be set</summary>

```
JWT_AUTH_TOKEN_SECRET
JWT_REFRESH_TOKEN_SECRET
JWT_ISSUER
JWT_AUDIENCE
JWT_TOKEN_EXPIRY_IN_MINUTES
JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES
PASSWORD_RESET_TOKEN_EXPIRY_IN_MINS
PASSWORD_SALT_HASH_ROUNDS
TOKEN_HASH_SECRET
```

</details>

By default, new installation of Flowise will require an admin setup, similar to how you have to setup a root user for your database initially.

<figure><img src="../.gitbook/assets/image (2).png" alt="" width="478"><figcaption></figcaption></figure>

After setting up, user will be brought to Flowise dashboard. From the left side bar, you will see User & Workspace Management section. A default workspace was automatically created.

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Creating Workspace

To create a new Workspace, click Add New:

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

You will see yourself added as the Organization Admin in the workspace you created.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

To invite new users to the workspace, you need to create a Role first.

## Creating Role

Navigate to Roles in the left side bar, and click Add Role:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

User can specify granular control of permissions for each resources. The only exceptions are the resources in **User & Workspace Management** (Roles, Users, Workspaces, Login Activity). These are only available for Account Admin for now.

Here, we create an editor role which has access to everything. And another role with view-only permissions.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

## Invite User

<details>

<summary>For self-hosted enterprise, the following env variables must be set</summary>

```
INVITE_TOKEN_EXPIRY_IN_HOURS
SMTP_HOST
SMTP_PORT
SMTP_USER
SMTP_PASSWORD
```

</details>

Navigate to Users in left side bar, you will see yourself as the account admin. This is indicated by the person icon with a star:

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Click Invite User, and enter email to be invited, the workspace to be assigned, and the role as well.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Click Send Invite. The invited email will receive an invitation:

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Upon clicking the invitation link, invited user will be brought to a Sign Up page.

<figure><img src="../.gitbook/assets/image (10).png" alt="" width="463"><figcaption></figcaption></figure>

After signed up and logged in as invited user, you will be in the workspace assigned, and there will be no User & Workspace Management section:

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

If you are invited into multiple workspaces, you can switch to different workspaces from the top right dropdown button. Here we are assigned to Workspace 2 with **view only** permission. You can notice the Add New button for Chatflow is no longer visible. This ensure user can only view, not create, update nor delete. The same RBAC rules apply for API as well.

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Now, back to Account Admin, you will be able to see the users invited, their status, roles, and active workspace:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Account admin can also modify the settings for other users:

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

## Login Activity

Admin will be able to see every login and logout from all users:

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

## Creating item in Workspace

Every items created in a workspace, are isolated from another workspace. Workspaces are a way to logically group users and resources within an organization, ensuring separate trust boundaries for resource management and access control. It is recommended to create distinct workspaces for each team.

Here, we create a Chatflow named **Chatflow1** in **Workspace1**:

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

When we switch to **Workspace2**, **Chatflow1** will not be visible. This applies to every resources such as Agentflows, Tools, Assistants, etc.

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

The diagram below illustrates the relationship between organizations, workspaces, and the various resources associated with and contained within a workspace.

<figure><img src="../.gitbook/assets/Untitled-2024-10-19-0050.png" alt=""><figcaption></figcaption></figure>

## Sharing Credential

You can share credential to other workspaces. This allow users to reuse same set of credentials in different workspaces.

After creating a credential, Account Admin or user with Share Credential permission from the RBAC will be able to click Share:

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

User can select the workspaces to share the credential with:

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

Now, switch to the workspace where the credential was shared, you will see the Shared Credential. User is not able to edit shared credential.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

## Deleting a Workspace

Currently only Account Admin can delete workspaces. By default, you are not able to delete a workspace if there are still users within that workspace.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

You will need to unlink all of the invited users first. This allow flexibility in case you just want to remove certain users from a workspace. Note that Organization Owner who created the workspace is not able to be unlinked from a workspace.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

After unlinking invited users, and the only user left within the workspace is the Organization Owner, delete button is now clickable:

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Deleting a workspace is an irreversible action and will cascade delete all items within that workspace. You will see a warning box:

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

After deleting a workspace, user will fallback to the Default workspace. Default workspace that was automatically created at the start is not able to be deleted.
