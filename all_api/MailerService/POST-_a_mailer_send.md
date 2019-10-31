






 
Send an email to a user or any email address  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Attachments** |  | _array_ |   
**Cc** |  | _array_ |   
**ContentHtml** |  | _string_ |   
**ContentMarkdown** |  | _string_ |   
**ContentPlain** |  | _string_ |   
**DateSent** |  | _string_ |   
**From** |  | _#/definitions/mailerUser_ |   
**Retries** |  | _integer_ |   
**Sender** |  | _#/definitions/mailerUser_ |   
**Subject** |  | _string_ |   
**TemplateData** |  | _object_ |   
**TemplateId** |  | _string_ |   
**ThreadIndex** |  | _string_ |   
**ThreadUuid** | Could be used for Re: ... conversations | _string_ |   
**To** |  | _array_ |   
**sendErrors** |  | _array_ |   


### Body Example
```
{
  "Attachments": [
    "string"
  ],
  "Cc": [
    {
      "Address": "string",
      "Language": "string",
      "Name": "string",
      "Uuid": "string"
    }
  ],
  "ContentHtml": "string",
  "ContentMarkdown": "string",
  "ContentPlain": "string",
  "DateSent": "string",
  "From": {
    "Address": "string",
    "Language": "string",
    "Name": "string",
    "Uuid": "string"
  },
  "Retries": 10,
  "Sender": {
    "Address": "string",
    "Language": "string",
    "Name": "string",
    "Uuid": "string"
  },
  "Subject": "string",
  "TemplateData": {},
  "TemplateId": "string",
  "ThreadIndex": "string",
  "ThreadUuid": "string",
  "To": [
    {
      "Address": "string",
      "Language": "string",
      "Name": "string",
      "Uuid": "string"
    }
  ],
  "sendErrors": [
    "string"
  ]
}
```






### Response Example (200)
Response Type /definitions/mailerSendMailResponse

```
{
  "Success": true
}
```


