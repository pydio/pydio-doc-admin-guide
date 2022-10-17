This page collects information about noticeable changes to take care when upgrading to Cells new major versions.

## Cells v4

There are a couple of important notes during this upgrade :

 - Hydra JWKs will be regenerated in the DB, with the effect of invalidating all existing authentication tokens. You will be logged out after upgrade, and if you are using Personal Access Tokens, you will have to regenerate new ones.
 - Cells Sites Bind URL should not use a domain name but should declare a binding PORT, eventually including the Network Interface IP. If you have connection issue after upgrade, make sure to edit sites (cells configure sites) to bind to e.g. 0.0.0.0:8080 instead of domain.name:8080


