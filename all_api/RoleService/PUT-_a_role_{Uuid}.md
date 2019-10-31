






 
Create or update a Role  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**AutoApplies** |  | _array_ |   
**ForceOverride** |  | _boolean_ |   
**GroupRole** |  | _boolean_ |   
**IsTeam** |  | _boolean_ |   
**Label** |  | _string_ |   
**LastUpdated** |  | _integer_ |   
**Policies** |  | _array_ |   
**PoliciesContextEditable** |  | _boolean_ |   
**UserRole** |  | _boolean_ |   
**Uuid** |  | _string_ |   


### Body Example
```
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
```






### Response Example (200)
Response Type /definitions/idmRole

```
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
```


