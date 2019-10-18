Pydio Cells is compatible with Azure Blob Storage, you can connect your pydio datasources to your azure blob storage.

### Azure Blob Storage datasource

[:image-popup:4_connecting_your_datasource/datasource_config/azure_ds_interface.png]

To connect your datasource to the azure blob storage you need prerequisites listed below.

Look for the following menu in azure interface it's located in **All Services** > **Storage Account**, then create or choose one, after that look at the examples below to find what you need.

[:image-popup:4_connecting_your_datasource/datasource_config/azure_access_key.png]

in this menu you can find your **Azure account name** which corresponds to **Storage account name** and the **Azure Secret Key** which is the field `key` in either `key1` or `key2`.

Then found the bucket name, which are called `blob` in azure, you can find them in the following interface (that will also be used to create blobs).

[:image-popup:4_connecting_your_datasource/datasource_config/blob_azure.png]

- **Bucket name**: the name of the blob/bucket on the remote azure storage.
- **Azure Account Name**: your azure storage account name.
- **Azure Secret Key**: the secret key, is your storage access key/
- **Internal Path**: Additional Path appendended to the bucket name when querying the remote storage.
