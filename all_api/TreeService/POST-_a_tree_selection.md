






### [POST] /a/tree/selection  
Create a temporary selection for further action (namely download)  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Nodes** |  | _array_ |   
**Persist** |  | _boolean_ |   
**TargetAction** |  | _string_ |   


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
  ],
  "Persist": true,
  "TargetAction": "string"
}
```






### Response Example (200)
Response Type /definitions/restCreateSelectionResponse

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
  "SelectionUUID": "string"
}
```


