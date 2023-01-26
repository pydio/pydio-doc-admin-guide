Cells Enterprise 2.2 provides a new SFTP gateway that exposes users workspaces through the SFTP protocol. This page describes the steps required
to configure and enable the gateway.

### Creating server SSH keys

The first step is to create the server private key. Most common way is to use 
openSSL tool `ssh-keygen` to generate a keypair:

```SH
$> ssh-keygen -t rsa -f id_rsa
```
This will generate two files inside the current folder: private key `id_rsa` and public key `id_rsa.pub`. The path to the private key file will be required for the next step. 

### Enabling SFTP Gateway

SFTP Gateway must be enabled manually by editing the configuration file `$CELLS_WORKING_DIR/pydio.json`.

In the "services" section, add a JSON section for `pydio.gateway.sftp` 

```JSON
{
  "services": {
    [...]
    "pydio.gateway.sftp": {
      "enabled": true,        
      "hostKeyFile": "/path/to/id_rsa",
      "port": 2022,
      "tmpDir": "/path/to/custom/temp/directory",
      "tmpFilesLifespan": "15m"
    }
  }
}
```

Configuration values are described below.

|Name|Type|Mandatory|Default|Description|
|---|---|---|---|---|
|enabled|boolean|true| false |Start sFTP Gateway|
|hostKeyFile|string|true| |Full path to the generated private key|
|port|integer|false|2022|Port used to expose the SFTP Gateway|
|tmpDir|string|false|os.TempDir()|Temporary directory used to store uploaded files (see Limitations below)|
|tmpFilesLifespan|duration|false|"15m"|Time for cleaning temporary files that are kept for resumed uploads. Format is a golang duration.|

Once JSON is updated, just restart Cells Enterprise.

### Connecting to SFTP

Assuming you are using the default configuration, SFTP is accessed through the custom 2022 port. Note that as it is differing from HTTP protocol, it does
not go through Cells main Gateway. 

Create a Personal Token for a given user. For example, generating a token for user "admin" that will expire in 24h:

```SH
$> ./cells-enterprise admin user token -u admin -e 24h
✔ This token for admin will expire on 2021-01-28 14:36:32.206538 +0100 CET m=+86400.243984260.
✔ suDOydSgC-9myGgG27KZPqycDoO--jI_YIJCgvF7Qh0.ciN0O0gLx4xmnsC6BVH6ofivOSSbJoQa3qYowsOuswk
⚠ Make sure to secure it as it grants access to the user resources!
```

Once you have this token, you can use any SFTP client to connect with USERNAME/TOKEN as login/password.

### Limitations

SFTP protocol uploads files in a chunk-based, random-order mode. For this reason a temporary copy
of the file is created on the server **local filesystem**, before being transferred to the underlying datasource.

This is done in a temporary directory. Therefore if you plan to upload huge files via SFTP, **you must make sure
that the OS "tmp" location has enough available storage.** You can customize this location using the "tmpDir" parameter.