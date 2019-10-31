






 
List meta for a list of nodes, or a full directory using /path/* syntax  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**AllMetaProviders** |  | _boolean_ |   
**Limit** |  | _integer_ |   
**NodePaths** |  | _array_ |   
**NodeUuids** |  | _array_ |   
**Offset** |  | _integer_ |   
**Versions** |  | _boolean_ |   


### Body Example
```
{
  "AllMetaProviders": true,
  "Limit": 10,
  "NodePaths": [
    "string"
  ],
  "NodeUuids": [
    "string"
  ],
  "Offset": 10,
  "Versions": true
}
```






### Response Example (200)
Response Type /definitions/restBulkMetaResponse

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
  ],
  "Pagination": {
    "CurrentOffset": 10,
    "CurrentPage": 10,
    "Limit": 10,
    "NextOffset": 10,
    "PrevOffset": 10,
    "Total": 10,
    "TotalPages": 10
  }
}
```


