
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v3. Looking for <a href="https://pydio.com/en/docs/cells/v4/quick-start">Pydio Cells v4 docs?</a></span>
</div>

Pydio cells enterprise is compatible with google cloud object storage, therefore you can have datasources that are connected to your google cloud storage buckets.

### Google Cloud Storage datasource

Start by generating credentials if you do not have them,
for that, open the menu on your GCS dashboard (the top left burger) and then go to the following section **APIs & Services > Credentials**.

[:image-popup:4_connecting_your_storage/datasource_config/gcs_create_credentials.png]

Now click on **Create credentials** and select **Service account key**:

* Service account : You can use the default one.
* Key type : Choose **JSON**

then click on **Create**, it will generate and download automatically a JSON file, (you will it need later to configure your datasource).

Assuming that you already have created your bucket, here's how you connect a GCS bucket to a Cells datasource:

[:image:4_connecting_your_storage/datasource_config/gcs_ds_interface.png]

* Bucket Name : put your bucket name.
* GCS JSON Credentials: put the content of the file that we generated above with your gcs account.
* Internal path : you can make the datasource point to a specific folder inside the bucket.
