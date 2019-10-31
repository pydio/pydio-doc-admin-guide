






### [POST] /a/user-meta/tags/{Namespace}  
Add a new value to Tags for a given namespace  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Namespace** |  | _string_ |   
**Tag** |  | _string_ |   


### Body Example
```
{
  "Namespace": "string",
  "Tag": "string"
}
```






### Response Example (200)
Response Type /definitions/restPutUserMetaTagResponse

```
{
  "Success": true
}
```


