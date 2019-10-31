






### [POST] /a/meta/delete/{NodePath}  
Delete metadata of a given node  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Namespace** |  | _array_ |   
**NodePath** |  | _string_ |   


### Body Example
```
{
  "Namespace": [
    "string"
  ],
  "NodePath": "string"
}
```






### Response Example (200)
Response Type /definitions/treeNode

```
{
  "AppearsIn": [
    {
      "Path": "string",
      "WsLabel": "string",
      "WsUuid": "string"
    }
  ],
  "Commits": [
    {
      "Data": "string",
      "Description": "string",
      "Event": {
        "Metadata": {},
        "Optimistic": true,
        "Silent": true,
        "Source": "[Recursive structure]",
        "Target": "[Recursive structure]",
        "Type": "string"
      },
      "MTime": "string",
      "OwnerUuid": "string",
      "Size": "string",
      "Uuid": "string"
    }
  ],
  "Etag": "string",
  "MTime": "string",
  "MetaStore": {},
  "Mode": 10,
  "Path": "string",
  "Size": "string",
  "Type": "string",
  "Uuid": "string"
}
```


