### Datasource encryption

When creating a datasource admin can choose to enable encryption to have files store in the source encrypted.

To do so the "**encryption**" option must be enable in the datasource creation/edition form. When enabled the admin is able to select the key to use for file encryption.

##### Keys

Keys are generated from a secret pass phrase the the admin enter or can be imported from another **Cells** instance.

In the admin left panel click "**Storage**" in the "**Datamanagement**" section. And at the bottom of the page you can create/import encryption key.

* Key creation form

click on the create key button to generate a new key. Fill the form using

**key inditifier**: unique text that as key ID

**key label**: the name of key as it appear in the list

* Key export

When exporting a key a pass phrase is required to protect it in case the admin lose it.

* key import

To import a key the admin must provide the pass phrase he entered during export.  


#### Enable encryption

Back to the datasource create/edit form, after the admin enables the **encryption** option generated or imported key can be selected to encrypt to encrypt the datasource. 