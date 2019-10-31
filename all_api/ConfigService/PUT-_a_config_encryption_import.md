






 
Import a previously exported master key, requires the password created at export time  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**Key** |  | _#/definitions/encryptionKey_ |   
**Override** |  | _boolean_ |   
**StrPassword** |  | _string_ |   


### Body Example
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
  },
  "Override": true,
  "StrPassword": "string"
}
```






### Response Example (200)
Response Type /definitions/encryptionAdminImportKeyResponse

```
{
  "Success": true
}
```


