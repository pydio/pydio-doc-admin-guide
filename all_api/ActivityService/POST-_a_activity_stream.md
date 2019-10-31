






### [POST] /a/activity/stream  
Load the the feeds of the currently logged user  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**AsDigest** |  | _boolean_ |   
**BoxName** |  | _string_ |   
**Context** |  | _#/definitions/activityStreamContext_ |   
**ContextData** |  | _string_ |   
**Language** |  | _string_ |   
**Limit** |  | _string_ |   
**Offset** |  | _string_ |   
**PointOfView** |  | _#/definitions/activitySummaryPointOfView_ |   
**StreamFilter** |  | _string_ |   
**UnreadCountOnly** |  | _boolean_ |   


### Body Example
```
{
  "AsDigest": true,
  "BoxName": "string",
  "Context": "string",
  "ContextData": "string",
  "Language": "string",
  "Limit": "string",
  "Offset": "string",
  "PointOfView": "string",
  "StreamFilter": "string",
  "UnreadCountOnly": true
}
```






### Response Example (200)
Response Type /definitions/activityObject

```
{
  "accuracy": "[Unknown Type number]",
  "actor": "[Recursive structure]",
  "altitude": "[Unknown Type number]",
  "anyOf": "[Recursive structure]",
  "attachment": "[Recursive structure]",
  "attributedTo": "[Recursive structure]",
  "audience": "[Recursive structure]",
  "bcc": "[Recursive structure]",
  "bto": "[Recursive structure]",
  "cc": "[Recursive structure]",
  "closed": "string",
  "content": "[Recursive structure]",
  "context": "[Recursive structure]",
  "current": "[Recursive structure]",
  "deleted": "string",
  "duration": "string",
  "endTime": "string",
  "first": "[Recursive structure]",
  "formerType": "string",
  "generator": "[Recursive structure]",
  "height": 10,
  "href": "string",
  "hreflang": "string",
  "icon": "[Recursive structure]",
  "id": "string",
  "image": "[Recursive structure]",
  "inReplyTo": "[Recursive structure]",
  "instrument": "[Recursive structure]",
  "items": [
    "[Recursive structure]"
  ],
  "jsonLdContext": "string",
  "last": "[Recursive structure]",
  "latitude": "[Unknown Type number]",
  "location": "[Recursive structure]",
  "longitude": "[Unknown Type number]",
  "mediaType": "string",
  "name": "string",
  "next": "[Recursive structure]",
  "object": "[Recursive structure]",
  "oneOf": "[Recursive structure]",
  "origin": "[Recursive structure]",
  "partOf": "[Recursive structure]",
  "prev": "[Recursive structure]",
  "preview": "[Recursive structure]",
  "published": "string",
  "radius": "[Unknown Type number]",
  "rel": "string",
  "relationship": "[Recursive structure]",
  "replies": "[Recursive structure]",
  "result": "[Recursive structure]",
  "startTime": "string",
  "subject": "[Recursive structure]",
  "summary": "string",
  "tag": "[Recursive structure]",
  "target": "[Recursive structure]",
  "to": "[Recursive structure]",
  "totalItems": 10,
  "type": "string",
  "units": "string",
  "updated": "string",
  "url": "[Recursive structure]",
  "width": 10
}
```


