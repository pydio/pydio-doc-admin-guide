






 
Update/delete user meta  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**MetaDatas** |  | _array_ |   
**Operation** |  | _#/definitions/UpdateUserMetaRequestUserMetaOp_ |   


### Body Example
```
{
  "MetaDatas": [
    {
      "JsonValue": "string",
      "Namespace": "string",
      "NodeUuid": "string",
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
      "Uuid": "string"
    }
  ],
  "Operation": "string"
}
```






### Response Example (200)
Response Type /definitions/idmUpdateUserMetaResponse

```
{
  "MetaDatas": [
    {
      "JsonValue": "string",
      "Namespace": "string",
      "NodeUuid": "string",
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
      "Uuid": "string"
    }
  ]
}
```


