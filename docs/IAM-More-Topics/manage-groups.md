# Manage user groups

Groups are a way to categorize users. Consider a scenario where a tenant has multiple websites, with a group of FC Barcelona fans spread across all users. 

To provide additional context for the group, you may include custom data as a nested object.  

You can manage the groups through the [Groups API]().  

## Usage examples  

You can categorize users based on certain criteria and then look up necessary information or perform batch actions:  

- Congratulate FC Manchester City fans on another championship.
- Send your best Diwali wishes to Indian users.
- Search for experts within an HR or R&D group.

Depending on your use case, you may use the groups for research purposes. Consider fetching users, identifying their group memberships, and analyzing common patterns.  

## Step 1: Create groups  

To create a group, use [this API call]():  

```shell
curl -i -X POST \
  {{BASE_URL}}/v1/groups \
  -H 'Authorization: Bearer <APPLICATION_LEVEL_JWT_TOKEN>' \
  -H 'Content-Type: application/json' \
  # You may include optional data as a nested object (`custom_data`).
  # Consider providing additional context. 
  -d '{
    "name": "string",
    "description": "string",    
    "custom_data": {
      "key": "value"
    }
  }'
``` 

<details>  
<summary>Example output:</summary>  

```json
{
  "result": {
    "group_id": "4be2eff2574323ce",
    "name": "FC Barcelona fans",
    "description": "FC Barcelona fans across EMEA",
    "created_at": 1696481564,
    "updated_at": 1696481564,
    "custom_data": {
      "started_tracking": "2019-08-24T14:15:22Z"
    }
  }
}
```
</details>

## Step 2: Add users to groups  

To add users to a group, use [this API call]():  

```shell
curl -i -X POST \
  # Include the group ID in the path and an array of user IDs in the body. 
  '{{BASE_URL}}/v1/groups/{group_id}/members' \
  -H 'Authorization: Bearer <APPLICATION_LEVEL_JWT_TOKEN>' \
  -H 'Content-Type: application/json' \
  -d '{
    "user_ids": [
      "string"
    ]
  }'
```

## Step 3: Fetch groups of a user 

You can fetch a user's groups, for example, to provide them with appropriate access / user experience. 

You can get the user group names from [ID tokens]() by [retrieving claim]() `groups`. 

Also, you can [retrieve group IDs from user data]():  

```shell 
curl -i -X GET \
  # Include the user ID in the path. 
  '{{BASE_URL}}/v1/users/{user_id}/groups' \
  -H 'Authorization: Bearer <APPLICATION_LEVEL_JWT_TOKEN>'
```

This will return an array containing general information about all groups of the user:   

<details>  
<summary>Example output:</summary>  

```json
{
  "result": [
    {
      "group_id": "4be2eff2574323ce",
      "name": "FC Barcelona fans",
      "description": "FC Barcelona fans across EMEA",
      "created_at": 1696481564,
      "updated_at": 1696481987,
      "custom_data": {}
    }
  ]
}
```
</details>

## Step 4: Fetch users of a group  

You can retrieve an array of all users within a group. For example, you can target a specific group in a campaign. Use [this API call]():  

```shell 
curl -i -X GET \
  # Include the group ID in the requested URL. 
  '{{BASE_URL}}/v1/groups/{group_id}/members' \
  -H 'Authorization: Bearer <APPLICATION_LEVEL_JWT_TOKEN>'
```

The output will include entire user profiles.  

<details>  
<summary>Example output:</summary>  

<embed src="/guides/reusables/user/_user_group_example.md" />

</details> 

## Next steps  

Next steps depend on your requirements. Using Groups API, you can:  

- [Retrieve all groups]()  
- [Get]() or [update]() general information about a group  
- [Remove users from a group]()
- [Delete a group]()  

See the [Groups API documentation]() for detailed information.  