






 
Search Acls  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**CountOnly** |  | _boolean_ |   
**GroupBy** |  | _integer_ |   
**Limit** |  | _string_ |   
**Offset** |  | _string_ |   
**Operation** |  | _#/definitions/serviceOperationType_ |   
**Queries** |  | _array_ |   


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
      "Actions": [
        {
          "Name": "string",
          "Value": "string"
        }
      ],
      "NodeIDs": [
        "string"
      ],
      "RoleIDs": [
        "string"
      ],
      "WorkspaceIDs": [
        "string"
      ],
      "not": true
    }
  ]
}
```






### Response Example (200)
Response Type /definitions/restACLCollection

```
{
  "ACLs": [
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
  ],
  "Total": 10
}
```


