






### [POST] /a/activity/subscribe  
Manage subscriptions to other users/nodes feeds  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Events** |  | _array_ |   
**ObjectId** |  | _string_ |   
**ObjectType** |  | _#/definitions/activityOwnerType_ |   
**UserId** |  | _string_ |   


### Body Example
```
{
  "Events": [
    "string"
  ],
  "ObjectId": "string",
  "ObjectType": "string",
  "UserId": "string"
}
```






### Response Example (200)
Response Type /definitions/activitySubscription

```
{
  "Events": [
    "string"
  ],
  "ObjectId": "string",
  "ObjectType": "string",
  "UserId": "string"
}
```


