## Google cloud storage datasource

Pydio cells enterprise is compatible with google cloud object storage, therefore you can have datasources that are connected to your google cloud storage buckets.

First you need to generate credentials if you dont have any of them,
open the menu on your GCS dashbord (the top left burger) and then go to the following section **APIs & Services > Credentials**.

[:image-popup:3_storage_data_and_metadata/datasource_config/gcs_interface.png]

[:image-popup:3_storage_data_and_metadata/datasource_config/gcs_interface.png]

then click on **Create credentials** and select **Service account key**:

* Service account : You can use the default one.
* Key type : Choose **JSON**

then click on **Create**, it will generate and download automatically a JSON file, (you will it need later to configure your datasource).

Assuming that you already have created your bucket, here's how you connect a GCS bucket to a Cells datasource:

[:image-popup:3_storage_data_and_metadata/datasource_config/gcs_ds_interface.png]

* Bucket Name : put your bucket name.
* GCS JSON Credentials: put the content of the file that we generated above with your gcs account.
* Interal path : you can make the datasource point to a specific folder inside the bucket.
