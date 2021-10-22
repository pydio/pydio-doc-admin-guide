@TODO


## Create datasource

DataSources are mount points allowing you to attach actual storage to the application. First select the way data is organized inside storage.

- Flat Storage: The data becomes completely flat without a tree structure, on your file system all the files are named with UUIDs.
- Importing Existing Data: This allows you to keep an existing data structure on your file system.
- Cells Internals: This is used for specific data only related to Cells (such as Thumbnails and versions).

### 1) Identifier

How your storage will be identified on Cells, only **alphanumeric** s3 DNS compliant characters are allowed.

//TODO that is because internally Cells will add a s3 layer between Cells and the data.

### 2) Storage Type

Select the type of storage, for detailed information please refer to the [administration guide](./create-datasources).

### 3) Data Lifecycle

Setting up a versioning policy enables you to configure how Cells act with file versioning, how many versions are kept, how long we keep the old versions and many other options.

You can create your own [versioning policies](./versioning-policies) and configure how you want to handle your versions.

> File versions are stored inside the **Internal DataSource** named `versions`

[Encryption](./encryption) at rest can be used to make sure files are never stored in clear format inside the storage, files will be encrypted with a key and only be readable when decrypted with the same key.

**DISCLAIMER: Keys are stored in the default database and if they ever get lost the encrypted data could not be recovered.**

### 4) Advanced Options

Advanced options should only be used for specific cases.

//TODO


