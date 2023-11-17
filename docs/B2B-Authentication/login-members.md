# Log in members   

## Step 1: Set up organization  

At this stage, you need to set up your organization(s). You can add an organization both through the Admin Portal (**Identity management** > **Organizations**) or via the [Organization API](/openapi/user/organizations/).

Here's an example request that creates an organization:  

```shell
curl -i -X POST \
  {{BASE_URL}}/v1/organizations \
  -H 'Authorization: Bearer <YOUR_JWT_HERE>' \
  -H 'Content-Type: application/json' \
  # Required parameters: organization name, domain, and app ID(s)
  -d '{
    "name": "string",
    "domain": "string",
    "app_ids": [
      "string"
    ]
  }'
```

When the platform saves an organization, it assigns it a unique ID. Later on, you may add more organizations, for example, your customers or partners.  

## Step 2: Enable authentication  


You have the following authentication options:  

Option to implement | Description 
---|---
Internal app authentication | Use the app's intenal authentication, but pass the `org_id` value in the response.  
Single Sign-on (SSO) federation | If you have an account with a third-party IDP (Identity Provider), implement [OIDC](/guides/user/sso_login_oidc_idp.md) or [SAML](/guides/user/sso_login_saml_idp.md) SSO federation. 
Hosted application | Implement Transmit's hosted app experience.   


## Step 3: Assign organization administrators 


Now you need to add an organization admin. An organization admin can use the org admin portal: add organization members and management apps, view available roles and assign them to the members. 

You can add an org admin by invoking [this API call](/openapi/user/members/#operation/createOrAssignMember) or through the Admin Portal (**Identity Management** > **Organizations**, then open the **Members** tab on your org's page). 

Here's an example request that adds an org admin:  

```console 
curl -i -X POST \
  '{{BASE_URL}}/v1/organizations/{organization_id}/members' \
  -H 'Authorization: Bearer <YOUR_JWT_HERE>' \
  -H 'Content-Type: application/json' \
  # The payload provides minimum data for a new user: unique identifier (email in this case) and a role ID.
  # For this role, role id equals role name: 'Organization Admin'
  -d '{
    "email": "string",
    "role_ids": [
      "Organization Admin"
    ]
  }'

```

## Step 4: Invite members


You can invite members both on the application and organization levels. 

On the **application level**, you can use [this API request](/openapi/user/members/#operation/createOrAssignMember) or the Admin Portal (**Identity Management** > **Organizations**, then open the **Members** tab on your organization's page). 

To invite members on the **organization level**, open the organization portal, go to **Members**, click **+Add member**, and enter the member's details.  

Required parameters: 

- Member's email and/or phone number  
- Assigned app roles  

Here's an example request that creates a member:  

```shell
curl -i -X POST \
  # Include the org ID in the path. 
  '{{BASE_URL}}/v1/organizations/{organization_id}/members' \
  -H 'Authorization: Bearer <YOUR_JWT_HERE>' \
  -H 'Content-Type: application/json' \
  -d '{
    # Primary email
    "email": "string",
    # User identifier. Required unless you want to use the email as primary ID. 
    "username": "string",
    "credentials": {
      # New password (required)
      "password": "string",
      # True means this is a temp password: the user has to change it upon first login.
      "force_replace": true
    },
    # At least one role ID is required. 
    "role_ids": [
      "string"
    ],
    # Sends an invite to the specified email address. 
    "send_invite": true
  }'
```

In addition to inviting a member via email, you can create a UI for self-serving signup and allow the users to register on their own.  

When saving the user, the platform marks their account as `Pending` and sends the user a confirmation message. When the user clicks the confirmation link and starts using the account, the platform marks the account as `Active`.  

!!! note
    You can invite a user to multiple organizations.
    If you use a hosted application, users who have memberships in multiple organizations 

Visit [Manage users]() for additional information.  

## Next steps  

### Provide role-based access  

Roles are basically collections of app permissions. By assigning app roles to users, you authorize them to use the app's functionality.  

You can control which app roles are available to organizations through role groups. 

**Applications** can assign user roles via API and through the Admin Portal.  
**Organizations** can assign member roles via the **organization admin portal**.  

To find out how to manage role-based access, see [Manage and assign roles](manage-roles.md).  

### Update personal information  

You can add information to user profiles by yourself, through the [Identity Management API]() or in the Admin Portal. Alternatively, you may implement a profile update page using [this API call]() and invite the members to update their profiles on their own.  

<!-- ### Set up authentication experience  

Our B2B solution enables you to enhance users' authentication experiences, and oversee organizational information. The solution is compatible with third-party social and business Identity Providers (IDPs) such as Google, Okta, and others. Or you can use it with Transmit's own Identity Provider for added benefits.

### More capabilities for Transmit IDP consumers  

Many of the things you can do next depend on your business configuration and technical assets. For example, if you use Transmit's hosted app, you can use the Experiences Manager that enables you to define the authentication journey through parameters:

- User identifier 
- Required personal information fields   
- Primary and secondary authentication methods: password, passcode, SMS OTP, email OTP, email magic link, socials 
- Multi-factor authentication options  

Talk to your account manager to find out how to take advantage of these technical capabilities.    -->

## Learn more  

- [Organization API]()  
- [Members API]()  
- [Users API]()  