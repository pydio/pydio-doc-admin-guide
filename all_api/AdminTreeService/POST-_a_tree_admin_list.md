






### [POST] /a/tree/admin/list  
List files and folders starting at the root (first level lists the datasources)  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Ancestors** |  | _boolean_ |   
**FilterType** |  | _#/definitions/treeNodeType_ |   
**Limit** |  | _string_ |   
**Node** |  | _#/definitions/treeNode_ |   
**Offset** |  | _string_ |   
**Recursive** |  | _boolean_ |   
**WithCommits** |  | _boolean_ |   
**WithVersions** |  | _boolean_ |   


### Body Example
```
{
  "Ancestors": true,
  "FilterType": "string",
  "Limit": "string",
  "Node": {
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
  },
  "Offset": "string",
  "Recursive": true,
  "WithCommits": true,
  "WithVersions": true
}
```






### Response Example (200)
Response Type /definitions/restNodesCollection

```
{
  "Children": [
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
  "Parent": {
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
}
```


