






### [PUT] /a/jobs/user  
Send Control Commands to one or many jobs / tasks  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Cmd** |  | _#/definitions/jobsCommand_ |   
**JobId** |  | _string_ |   
**OwnerId** |  | _string_ |   
**TaskId** |  | _string_ |   


### Body Example
```
{
  "Cmd": "string",
  "JobId": "string",
  "OwnerId": "string",
  "TaskId": "string"
}
```






### Response Example (200)
Response Type /definitions/jobsCtrlCommandResponse

```
{
  "Msg": "string"
}
```


