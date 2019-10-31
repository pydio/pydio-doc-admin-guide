






### [POST] /a/acl/bulk/delete  
Delete one or more ACLs  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Action** |  | _#/definitions/idmACLAction_ |   
**ID** |  | _string_ |   
**NodeID** |  | _string_ |   
**RoleID** |  | _string_ |   
**WorkspaceID** |  | _string_ |   


### Body Example
```
{
  "Action": {
    "Name": "string",
    "Value": "string"
  },
  "ID": "string",
  "NodeID": "string",
  "RoleID": "string",
  "WorkspaceID": "string"
}
```






### Response Example (200)
Response Type /definitions/restDeleteResponse

```
{
  "NumRows": "string",
  "Success": true
}
```


