## Azure Blob Storage

Pydio Cells is compatible with Azure Blob Storage, you can connect your pydio datasources to your azure blob storage.

[:image-popup:3_storage_data_and_metadata/datasource_config/azure_ds_interface.png]

To connect your datasource to the azure blob storage you need the following,

The menu in azure interface is located in **All Services** > **Storage Account**, then you can create or choose one, after that look at the examples below to find what you need.

[:image-popup:3_storage_data_and_metadata/datasource_config/azure_access_key.png]

in this menu you can find your **Azure account name** which is corresponding to **Storage account name** and the **Azure Secret Key** which is the field `key` in either `key1` or `key2`.

Then to found the bucket name, which is called blob in azure you can find it in the following interface ( that you will be using to create them also).

[:image-popup:3_storage_data_and_metadata/datasource_config/blob_azure.png]

- **Bucket name**: the name of the blob/bucket on the remote azure storage.
- **Azure Account Name**: your azure storage account name.
- **Azure Secret Key**: the secret key, is your storage access key/
- **Internal Path**: Additional Path appendended to the bucket name when querying the remote storage.
