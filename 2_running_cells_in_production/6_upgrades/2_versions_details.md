This page collects information about noticeable changes to take care when upgrading to Cells new major versions.

## Cells v4

Major codebase changes for embracing Go Modules, upgrading dependencies, and simplifying microservices framework to ease cluster deployments.

Upgrade is done by In-App Tool, but there are a couple of important notes:

 - As any major version, make sure to have a good backup of your MySQL database as well as your CELLS_WORKING_DIR content, except for the actual data but including the `.minio.sys` folder if possible. 

 - Hydra JWKs will be regenerated in the DB, with the effect of invalidating all existing authentication tokens. As a result, **you will be logged out after upgrade**, and if you are using Personal Access Tokens, you will have to regenerate new ones.  
 
 - **Cells Sites Bind URL should not use a domain name anymore**. It should declare a binding PORT eventually including the _Network Interface IP_. If you have connection issue after upgrade, make sure to edit sites to bind to e.g. `0.0.0.0:8080` instead of `domain.name:8080` (using `cells configure sites` or by simply editing the `pydio.json` file).

### Collation issues

While upgrading to Cells v4 from an older version, you might get this warning:

```sh
WARN	pydio.grpc.policy	[SQL] *****************************************************************************
WARN	pydio.grpc.policy	[SQL] 
WARN	pydio.grpc.policy	[SQL]   The following tables have a character set that does not match the default character set for the database...
WARN	pydio.grpc.policy	[SQL]   	{"name": "idm_policy_rel", "collation": "latin1_swedish_ci"}
WARN	pydio.grpc.policy	[SQL]   	{"name": "ladon_policy_subject", "collation": "latin1_swedish_ci"}
WARN	pydio.grpc.policy	[SQL]   	{"name": "idm_workspace_policies", "collation": "latin1_swedish_ci"}
# (... more warnings...)  
WARN	pydio.grpc.policy	[SQL]   It might be due to the database being migrated from another system or the default database having been updated.
WARN	pydio.grpc.policy	[SQL]   It could potentially lead to issues during upgrades so we you should pre-emptively fix the tables collations.
WARN	pydio.grpc.policy	[SQL]   You can find more information here : https://pydio.com/kb/...
WARN	pydio.grpc.policy	[SQL] 
WARN	pydio.grpc.policy	[SQL] *******************************************************************************
2022-10-26T11:06:08.982+0200	INFO	pydio.g
```

and/or this error:

```sh
ERROR	pydio.grpc.oauth	Stopping all migrations as some tables may have collations differing from the database defaults. This may break migrations and foreign keys.
```

This have been seen on some old instances that went through various OS upgrades and migrations and landed in a _not very clean_ state in terms of character sets and collation. This had no impact on the running instance in v3- versions but is problematic while running DB migrations to upgrade to Cells v4.  
Rather than risking to end up with a broken instance, we chose to prevent migration when we are in this case.

To solve this issue, you should:

- Stop Cells,
- Insure that no process is left (e.g. by running: `ps -afe | grep cells`),
- Perform a backup of your DB,
- Run following SQL commands:

```sh
# Create a custom script in /tmp folder
echo "SET FOREIGN_KEY_CHECKS=0;" > /tmp/upgrade.sql
mysql -N -upydio -p -e "SELECT CONCAT('ALTER TABLE ', tbl.TABLE_SCHEMA, '.', tbl.TABLE_NAME, ' CONVERT TO CHARACTER SET utf8mb4;') FROM INFORMATION_SCHEMA.TABLES tbl WHERE TABLE_SCHEMA='cells' AND TABLE_TYPE='BASE TABLE' AND TABLE_COLLATION NOT LIKE 'ascii%';" >> /tmp/upgrade.sql
echo "SET FOREIGN_KEY_CHECKS=1;" >> /tmp/upgrade.sql
# Run it against you DB
mysql -upydio -p cells < /tmp/upgrade.sql
```

You can then restart Cells, the errors and warnings should be gone and your migration should run without further issue. 

## Cells v3

Introducing new "Flat" datasource format and many Cells Flows new features.

Upgrade is straight-forward by using the In-App Tool.

## Cells v2

Synchronization rebooted and standard authentication development.

Upgrade is straight-forward by using the In-App Tool.