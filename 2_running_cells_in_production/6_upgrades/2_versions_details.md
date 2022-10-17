This page collects information about noticeable changes to take care when upgrading to Cells new major versions.

## Cells v4

Major codebase changes for embracing Go Modules, upgrading dependencies, and simplifying microservices framework to ease cluster deployments.

Upgrade is done by In-App Tool, but there are a couple of important notes:

 - As any major version, make sure to have a good backup of your MySQL database as well as your CELLS_WORKING_DIR content, except for the actual data but including the `.minio.sys` folder if possible. 


 - Hydra JWKs will be regenerated in the DB, with the effect of invalidating all existing authentication tokens. As a result, **you will be logged out after upgrade**, and if you are using Personal Access Tokens, you will have to regenerate new ones.  
 

 - **Cells Sites Bind URL should not use a domain name anymore**. It should declare a binding PORT eventually including the _Network Interface IP_. If you have connection issue after upgrade, make sure to edit sites to bind to e.g. `0.0.0.0:8080` instead of `domain.name:8080` (using `cells configure sites` or by simply editing the `pydio.json` file).

## Cells v3

Introducing new "Flat" datasource format and many Cells Flows new features.

Upgrade is straight-forward by using the In-App Tool.

## Cells v2

Synchronization rebooted and standard authentication development.

Upgrade is straight-forward by using the In-App Tool.