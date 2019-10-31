






 
Create or update a datasource  


### Body Parameters

Name | Description | Type | Required
---|---|---|---
**ApiKey** |  | _string_ |   
**ApiSecret** |  | _string_ |   
**CreationDate** |  | _integer_ |   
**Disabled** |  | _boolean_ |   
**EncryptionKey** |  | _string_ |   
**EncryptionMode** |  | _#/definitions/objectEncryptionMode_ |   
**LastSynchronizationDate** |  | _integer_ |   
**Name** |  | _string_ |   
**ObjectsBaseFolder** |  | _string_ |   
**ObjectsBucket** |  | _string_ |   
**ObjectsHost** |  | _string_ |   
**ObjectsPort** |  | _integer_ |   
**ObjectsSecure** |  | _boolean_ |   
**ObjectsServiceName** |  | _string_ |   
**PeerAddress** |  | _string_ |   
**StorageConfiguration** |  | _object_ |   
**StorageType** |  | _#/definitions/objectStorageType_ |   
**VersioningPolicyName** |  | _string_ |   
**Watch** |  | _boolean_ |   


### Body Example
```
{
  "ApiKey": "string",
  "ApiSecret": "string",
  "CreationDate": 10,
  "Disabled": true,
  "EncryptionKey": "string",
  "EncryptionMode": "string",
  "LastSynchronizationDate": 10,
  "Name": "string",
  "ObjectsBaseFolder": "string",
  "ObjectsBucket": "string",
  "ObjectsHost": "string",
  "ObjectsPort": 10,
  "ObjectsSecure": true,
  "ObjectsServiceName": "string",
  "PeerAddress": "string",
  "StorageConfiguration": {},
  "StorageType": "string",
  "VersioningPolicyName": "string",
  "Watch": true
}
```






### Response Example (200)
Response Type /definitions/objectDataSource

```
{
  "ApiKey": "string",
  "ApiSecret": "string",
  "CreationDate": 10,
  "Disabled": true,
  "EncryptionKey": "string",
  "EncryptionMode": "string",
  "LastSynchronizationDate": 10,
  "Name": "string",
  "ObjectsBaseFolder": "string",
  "ObjectsBucket": "string",
  "ObjectsHost": "string",
  "ObjectsPort": 10,
  "ObjectsSecure": true,
  "ObjectsServiceName": "string",
  "PeerAddress": "string",
  "StorageConfiguration": {},
  "StorageType": "string",
  "VersioningPolicyName": "string",
  "Watch": true
}
```


