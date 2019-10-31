






 
Create or update a workspace  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Attributes** |  | _string_ |   
**Description** |  | _string_ |   
**Label** |  | _string_ |   
**LastUpdated** |  | _integer_ |   
**Policies** |  | _array_ |   
**PoliciesContextEditable** |  | _boolean_ |   
**RootNodes** |  | _object_ |   
**RootUUIDs** |  | _array_ |   
**Scope** |  | _#/definitions/idmWorkspaceScope_ |   
**Slug** |  | _string_ |   
**UUID** |  | _string_ |   


### Body Example
```
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
```






### Response Example (200)
Response Type /definitions/idmWorkspace

```
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
```


