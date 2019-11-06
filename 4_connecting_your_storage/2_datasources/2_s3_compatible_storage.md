### S3 Datasources

This connector allows using a remote S3-compliant storage as datasource for Pydio Cells, for instance you can connect your cells to a minio storage.

Under the hood, an object service is started in Gateway mode using the Api KEY/SECRET that you will provide.

[:image-popup:4_connecting_your_storage/datasource_config/s3_ds_interface.png]

- **Bucket name**: the name of the bucket on the remote storage where the data will be stored.
- **S3 Api Key**: the API key (can be seen as a login) that identifies you.
- **S3 Api Secret**: the API secret (can be seen as a password) that completes the identification process giving you the ability to store your data inside this bucket.
- **Internal Path**: Additional Path appended to the bucket name when querying the remote storage.
- **Custom Endpoint**: If the remote storage is Amazon S3, you can leave this empty. Otherwise, provide right here the http/https URL for to the s3-compatible storage.

## How to configure Amazon S3 storage or Minio

We provide you with 2 examples of connections with S3 storages, you can use them to understand how the datasources interact with the S3 connectors.

### Connect with Amazon S3 Storage

To connect your datasource to your S3 bucket you need to authenticate the S3 storage using credentials which are composed of an **accessKey** and a **secretKey**, in your amazon s3 instance they match as the following,

- **S3 Api key**: `access key id`
- **S3 Api secret**: `secret access key`
  
you can get them on your amazon aws account, here is a piece of documentation from the amazon doc that will point you to the right place, [https://aws.amazon.com/blogs/security/wheres-my-secret-access-key/](https://aws.amazon.com/blogs/security/wheres-my-secret-access-key/).

[:image-popup:4_connecting_your_storage/datasource_config/s3_credentials.png]

then once you have all of the informations and assuming that you have already created your bucket, these are the mandatory fields to get your datasource to work:

- **Bucket name**: your amazon S3 created bucket name.
- **S3 Api key**: the `access key id` that you retrieve as explained above
- **S3 Api secret**: the `secret access key` that you retrieve as explained above
  
(optional parameters)
- **Internal path**: you can make the datasource point to a specific "folder" in your s3 storage.

### Connect with Minio Storage

To connect with a minio storage, first you need to retrieve your minio server **AccessKey** and **SecretKey** ( there is many ways for that, you can also define them yourself), the details can be found inside [minio's documentation](https://docs.minio.io/docs/minio-quickstart-guide.html).

Then you will need to fill the fields as the following:

- **Bucket name**: pretty straightforward your bucket name that you created with minio.
- **S3 Api Key**: the `accessKey` that you retrieved as explained above.
- **S3 Api Secret**: the `secretKey` that you retrieved as explained above.
- **Internal Path**: if you want to point to a specific "folder" (sort of speaking) in your bucket (it's an optional parameter).
- **Custom Endpoint**: is the address where your Minio server is running, usually `<address>:9000`, for instance `http://192.168.0.116:9000`.