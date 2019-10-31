






### [POST] /a/config/encryption/export  
Export a master key for backup purpose, protected with a password  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**KeyID** |  | _string_ |   
**StrPassword** |  | _string_ |   


### Body Example
```
{
  "KeyID": "string",
  "StrPassword": "string"
}
```






### Response Example (200)
Response Type /definitions/encryptionAdminExportKeyResponse

```
{
  "Key": {
    "Content": "string",
    "CreationDate": 10,
    "ID": "string",
    "Info": {
      "Exports": [
        {
          "By": "string",
          "Date": 10
        }
      ],
      "Imports": [
        {
          "By": "string",
          "Date": 10
        }
      ]
    },
    "Label": "string",
    "Owner": "string"
  }
}
```


