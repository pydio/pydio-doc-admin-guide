






### [POST] /a/config/processes  
List running Processes, with option PeerId or ServiceName filter  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**PeerId** |  | _string_ |   
**ServiceName** |  | _string_ |   


### Body Example
```
{
  "PeerId": "string",
  "ServiceName": "string"
}
```






### Response Example (200)
Response Type /definitions/restListProcessesResponse

```
{
  "Processes": [
    {
      "ID": "string",
      "MetricsPort": 10,
      "ParentID": "string",
      "PeerAddress": "string",
      "PeerId": "string",
      "Services": [
        "string"
      ],
      "StartTag": "string"
    }
  ]
}
```


