# Manage and assign roles  

Roles are essentially collections of permissions that enable the members to use the application's features.  

## Step 1: Create roles  


Each organization provides default roles: `Organization member`, `Organization admin`, and `Organization creator`. 

If you need additional roles, you should add them on your own. You can either invoke [this Roles API call]() or use the Admin Portal (**Indentity Management** > **Roles**). The API call has the advantage of enabling you to specify the app permissions that belong to the role:

```shell
curl -i -X POST \
  '{{BASE_URL}}/v1/applications/{app_id}/roles' \
  -H 'Authorization: Bearer <YOUR_JWT_HERE>' \
  -H 'Content-Type: application/json' \
  -d '{
    "role_name": "string",
    "permissions": [
      "string"
    ],
    "description": "string",
    "display_name": "string"
  }'
```

Here's an example of information you can retrieve via the [Roles API]():  

```json
{
    "result": [
        {
            "role_id": "examplete8j03evryvi6w",
            "role_name": "AcmeCustomer",
            "permissions": [
                "searchForItems",
                "manageCart",
                "checkoutCart",
            ],
            "description": "Customers can buy products in our online store",
            "display_name": "AcmeCustomer",
            "app_id": "example4o9m1k7kkzj2uo"
        }
    ]
}
```

## Step 2: Add role groups   

Role groups are a way for applications to control which roles the organizations can assign to their members. 

A role group is a collection of roles an application can create and assign to organizations. Hence, the organizations can only choose from roles that belong to the assigned role groups.  

To set up a role group or add existing roles to it, you have the following options:  

- Invoke [this API call](/openapi/user/role-groups/#operation/createRoleGroup). 
- Go to the Admin Portal (**Identity Management** > **Roles**) and create a role group **Groups** tab.  

Here's an example API call that creates a role:  

```shell 
curl -i -X POST \
  # Include the app ID in the path.  
  '{{BASE_URL}}/v1/applications/{app_id}/role-groups' \
  -H 'Authorization: Bearer <YOUR_JWT_HERE>' \
  -H 'Content-Type: application/json' \
  # Required values: role IDs and name of the role group
  -d '{
    "role_ids": [
      "role_id1",
      "role_id2"
    ],
    "name": "My Group",
    "description": "My Group's description",
    "display_name": "string"
  }'
```

## Step 3: Assign roles to members  

<div class="badge-wrapper">
    <div class="badge">Application</div>
    <div class="badge">Organization</div>
</div>

To manage the members' access to an app, an organization can assign roles to its members. The organization can only choose the roles from the role groups that the application assigned to the organization. 
You can assign roles via API for [new members]() or [existing members](), and via the Admin Portal (**Identity Management** > **Organizations**, then open the **Members** tab on your organization's page). 

On the organization level, you can assign roles to new / existing members through the organization admin portal (go to **Members**). 

Here's an example API call that assigns roles to a new member:  

```shell
curl -i -X POST \
  # Include app Id, org ID, and user ID in the requested URL.
  '{{BASE_URL}}/v1/applications/{app_id}/organizations/{organization_id}/members/{user_id}/roles' \
  -H 'Authorization: Bearer <YOUR_JWT_HERE>' \
  -H 'Content-Type: application/json' \
  # Include the role IDs in the request body.  
  -d '{
    "role_ids": [
      "string1",
      "string2"
    ]
  }'
```

## Step 4: Retrieve member roles  

You can get members' roles from [identity]() and [access]() tokens, or via [Members API]().  

### Retrieving member roles from the tokens  

You can get role names from ID tokens and role ID from access tokens:  

1. Decode the tokens. 
2. Retrieve the values:  
    * From [ID token](): [Custom claim]() `role_values`
    * From [access token](): Claim `roles`  

Example `role_values` claim:  
```json
{ "role_values": ["author", "sales", "Organization Admin"] }
```

Example `roles` claim:  

```json
// This claim provides role IDs instead of role values
{ "roles": ["943abcdhu8ykuwdi2lmzm", "65vjd08hu8ykuwdi2lmzm", "Organization Admin"] }
```

### Retrieving member roles via API  

To get member roles via [Members API](), invoke [this API call]() call and retrieve the info from the output. 
Example API request:  

```shell
curl -i -X GET \
  # Include the org ID and the user ID in the requested URL. 
  '{{BASE_URL}}/v1/organizations/{organization_id}/members/{user_id}' \
  -H 'Authorization: Bearer <YOUR_JWT_HERE>'
```

Example API call output:  

```json
{
    "result": {
        "user_id": "exampleqytq5lehc68ruc",
        "created_at": 1689848110338,
        "updated_at": 1692891468793,
        // Other values
        "roles": [
            {
                "role_id": "123abcdhu8ykuwdi2lmzm",
                "role_name": "Author",
                "display_name": "author"
            },
            {
                "role_id": "123abcdujl1wwu11pms22",
                "role_name": "Sales",
                "display_name": "sales"
            },
            {
                "role_id": "Organization Admin",
                "role_name": "Organization Admin",
                "display_name": "Organization Admin"
            }
        ]
    }
}
```


### Fetch role permissions

You can fetch your app's permissions by sending [this API call]():  

```shell
curl -i -X GET \
  # Include the app ID in the requested URL. 
  '{{BASE_URL}}/v1/applications/{app_id}/roles' \
  -H 'Authorization: Bearer <YOUR_JWT_HERE>'
```

This returns an array of roles that may include permissions. Example output:  

```json
{
  "result": [
    {
      "role_id": "abc123dujl1wwu11pms22",
      "role_name": "AcmeCustomer",
      "permissions": [
        "manageCart",
        "checkoutCart"
      ],
      "app_id": "748abcdhu8yk231i2lmzm",
      "description": "Enables an authorized user to buy things at our online store.",
      "display_name": "AcmeCustomer"
    }
  ]
}
```

!!! note
    We recommend caching app permissions for each role. You may not update roles frequently and caching will prevent you from sending excessive calls to our servers. Make sure to set a reasonable cache time-to-live (TTL).

## Learn more  

- [Roles API]()
- [Members API]()  
