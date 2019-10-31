






 
Search a list of meta by node Id or by User id and by namespace  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**MetaUuids** |  | _array_ |   
**Namespace** |  | _string_ |   
**NodeUuids** |  | _array_ |   
**ResourceQuery** |  | _#/definitions/serviceResourcePolicyQuery_ |   
**ResourceSubjectOwner** |  | _string_ |   


### Body Example
```
{
  "MetaUuids": [
    "string"
  ],
  "Namespace": "string",
  "NodeUuids": [
    "string"
  ],
  "ResourceQuery": {
    "Any": true,
    "Empty": true,
    "Subjects": [
      "string"
    ]
  },
  "ResourceSubjectOwner": "string"
}
```






### Response Example (200)
Response Type /definitions/restUserMetaCollection

```
{
  "Metadatas": [
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


