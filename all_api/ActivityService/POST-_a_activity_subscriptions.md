






 
Load subscriptions to other users/nodes feeds  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**ObjectIds** |  | _array_ |   
**ObjectTypes** |  | _array_ |   
**UserIds** |  | _array_ |   


### Body Example
```
{
  "ObjectIds": [
    "string"
  ],
  "ObjectTypes": [
    "string"
  ],
  "UserIds": [
    "string"
  ]
}
```






### Response Example (200)
Response Type /definitions/restSubscriptionsCollection

```
{
  "subscriptions": [
    {
      "Events": [
        "string"
      ],
      "ObjectId": "string",
      "ObjectType": "string",
      "UserId": "string"
    }
  ]
}
```


