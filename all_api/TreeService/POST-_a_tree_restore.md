






### [POST] /a/tree/restore  
Handle nodes restoration from recycle bin  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Nodes** |  | _array_ |   


### Body Example
```
{
  "Nodes": [
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
  ]
}
```






### Response Example (200)
Response Type /definitions/restRestoreNodesResponse

```
{
  "RestoreJobs": [
    {
      "Label": "string",
      "NodeUuid": "string",
      "Uuid": "string"
    }
  ]
}
```


