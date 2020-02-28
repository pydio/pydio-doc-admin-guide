### S3 Datasources

This connector allows using a remote S3-compliant storage as datasource for Pydio Cells, for instance you can connect your cells to a minio storage.

Under the hood, an object service is started in Gateway mode using the Api KEY/SECRET that you will provide.

We provide you with 2 examples of connections with S3 storages, you can use them to understand how the datasources interact with the S3 connectors.

### Connect with Amazon S3 Storage

To connect your datasource to your S3 bucket you need to authenticate the S3 storage using credentials which are composed of an **accessKey** and a **secretKey**, in your amazon s3 instance they match as the following,

- **S3 Api key**: `access key id`
- **S3 Api secret**: `secret access key`
  
you can get them on your amazon aws account, here is a piece of documentation from the amazon doc that will point you to the right place, [https://aws.amazon.com/blogs/security/wheres-my-secret-access-key/](https://aws.amazon.com/blogs/security/wheres-my-secret-access-key/).

[:image-popup:4_connecting_your_storage/datasource_config/s3_credentials.png]

then once you have all of the informations and assuming that you have already created your bucket, these are the mandatory fields to get your datasource to work:

- **S3 Api key**: the `access key id` that you retrieve as explained above
- **S3 Api secret**: the `secret access key` that you retrieve as explained above
- **Buckets**: Once you have provided a Valid Key and Secret you will see appear a list of the buckets that you have access to and can select them with by clicking or with a regular expression.

_You can notice that we used a regex to select only the buckets starting with cells_

[:image:4_connecting_your_storage/datasource_config/datasource_amazon_s3.png]

### Connect with Minio Storage

To connect with a minio storage, you need to retrieve your minio server **AccessKey** and **SecretKey** ( there is many ways for that, you can also define them yourself), the details can be found inside [minio's documentation](https://docs.minio.io/docs/minio-quickstart-guide.html).

Then you will need to fill the fields as the following:

- **S3 Api Key**: the `accessKey` that you retrieved as explained above.
- **S3 Api Secret**: the `secretKey` that you retrieved as explained above.
- **Custom Endpoint**: is the address where your Minio server is running, usually `<address>:9000`, for instance `http://192.168.0.116:9000`.
- **S3 Api Region**: you can leave this empty
- **Buckets**: Once you have provided valid parameters for the previous fields you will see appear a list of buckets that you have access to and can select them with by clicking or with a regular expression.

[:image:4_connecting_your_storage/datasource_config/datasource_remote_s3.png]