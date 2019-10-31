






### [PUT] /a/user/{Login}  
Create or update a user  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Attributes** |  | _object_ |   
**GroupLabel** |  | _string_ |   
**GroupPath** |  | _string_ |   
**IsGroup** | Group specific data | _boolean_ |   
**Login** | User specific data | _string_ |   
**OldPassword** |  | _string_ |   
**Password** |  | _string_ |   
**Policies** |  | _array_ |   
**PoliciesContextEditable** |  | _boolean_ |   
**Roles** |  | _array_ |   
**Uuid** |  | _string_ |   


### Body Example
```
{
  "Attributes": {},
  "GroupLabel": "string",
  "GroupPath": "string",
  "IsGroup": true,
  "Login": "string",
  "OldPassword": "string",
  "Password": "string",
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
  ],
  "Uuid": "string"
}
```






### Response Example (200)
Response Type /definitions/idmUser

```
{
  "Attributes": {},
  "GroupLabel": "string",
  "GroupPath": "string",
  "IsGroup": true,
  "Login": "string",
  "OldPassword": "string",
  "Password": "string",
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
  ],
  "Uuid": "string"
}
```


