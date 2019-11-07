<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP), EOL end 2019. Time to move to <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells</a>!</span>
</div>

# Cloud drivers : s3, hpcloud #

Naturally, with the high rise of the Object-Storage systems, particularly in the cloud, a couple of drivers may help you in getting access to this kind of storage. Currently access.s3 and access.hpcloud are the only ones available.

Disclaimer : If you installed Pydio by the linux repository ( apt / yum ), you must install the "pydio-plugin-access.s3" adn the "pydio-plugin-metastore.s3" package before.

The access.swift should be generalized to an open-stack Swift driver, as they are actually using this technology. It requires the PHP Swift Bindings.

The S3 driver uses the "Better AS3 Stream Wrapper" class to access the amazon Simple Storage System. You must configure the  API & SECRET KEYS, the REGION to query, the CONTAINER you want to browse and the API Version ( 2 or 3 ). Optionally, you can set the "Signature Version" of the API. Using this kind of storage driver is handy for cloud architectures, and more features should be developed around it. Worth noting, the "Metastore.S3" is a specific metadata storage implementation that goes along with this driver, using directly the S3 metadata system.

[:image-popup:4_setup_workspaces_and_users/workspaces_object_storages.png]
