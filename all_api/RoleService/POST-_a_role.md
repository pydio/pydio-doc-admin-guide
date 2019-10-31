






 
Search Roles  


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
      "HasAutoApply": true,
      "IsGroupRole": true,
      "IsTeam": true,
      "IsUserRole": true,
      "Label": "string",
      "Uuid": [
        "string"
      ],
      "not": true
    }
  ],
  "ResourcePolicyQuery": {
    "Type": "string",
    "UserId": "string"
  }
}
```






### Response Example (200)
Response Type /definitions/restRolesCollection

```
{
  "Roles": [
    {
      "AutoApplies": [
        "string"
      ],
      "ForceOverride": true,
      "GroupRole": true,
      "IsTeam": true,
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
      "UserRole": true,
      "Uuid": "string"
    }
  ]
}
```


