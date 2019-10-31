






### [POST] /a/workspace  
Search workspaces on certain keys  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**CountOnly** |  | _boolean_ |   
**GroupBy** |  | _integer_ |   
**Limit** |  | _string_ |   
**Offset** |  | _string_ |   
**Operation** |  | _#/definitions/serviceOperationType_ |   
**Queries** |  | _array_ |   
**ResourcePolicyQuery** |  | _#/definitions/restResourcePolicyQuery_ |   


### Body Example
```
{
  "CountOnly": true,
  "GroupBy": 10,
  "Limit": "string",
  "Offset": "string",
  "Operation": "string",
  "Queries": [
    {
      "description": "string",
      "label": "string",
      "not": true,
      "scope": "string",
      "slug": "string",
      "uuid": "string"
    }
  ],
  "ResourcePolicyQuery": {
    "Type": "string",
    "UserId": "string"
  }
}
```






### Response Example (200)
Response Type /definitions/restWorkspaceCollection

```
{
  "Total": 10,
  "Workspaces": [
    {
      "Attributes": "string",
      "Description": "string",
      "Label": "string",
      "LastUpdated": 10,
      "Policies": [
        {
          "Action": "string",
          "Effect": "string",
          "JsonConditions": "string",
          "Resource": "string",
          "Subject": "string",
          "id": "string"
        }
      ],
      "PoliciesContextEditable": true,
      "RootNodes": {},
      "RootUUIDs": [
        "string"
      ],
      "Scope": "string",
      "Slug": "string",
      "UUID": "string"
    }
  ]
}
```


