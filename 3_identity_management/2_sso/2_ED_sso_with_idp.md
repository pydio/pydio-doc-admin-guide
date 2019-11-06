
As described in the SSO introduction, Cells has the ability to work as both a Service Provider, consuming authentication from third-party services using the *OIDC* and *SAML2.0* protocols, and as an Identity Provider, providing authentication tokens for other applications. 

This article describes the first case, by introducing generic interactions between Cells and external Identity Providers (concrete implementations are detailed in the Knowledge Base). 

## More reading...

Before going further, the following documents may be a good read to properly understand the underlying concepts:

- OpenID Connect (https://openid.net/connect/)
- OAuth 2.0 Authorization Framework (https://tools.ietf.org/html/rfc6749)
- Security Assertion Markup Language (SAML) (http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0-cd-02.html)

## Involved Parties

When configuring Cells with external providers, please keep in mind that you are working in an environment where serveral entities must know each other and exchange security information via public network. You must know the role of each entity and the protocol required to exchange information in a specific order. Although OIDC and SAML are different frameworks, they have following common parties:

- **Users**: the owner of the Cells data (_resource_), whose identification information is owned by the Identity Provider Server. Before accessing to their resources in Cells, users must identify at Identity Provider Server then authorize Cells to access to resource on behalf.
- **User Agent**: the client application/device used by users to connect to Cells. It's can be a web browser, sync client application or mobile application.
- **Cells**: the _Resource Server_ in OIDC, or _Relaying Party_ in SAML
- **Identity Provider Server**: Depending on the framework, this server provides several services such as, authentication, identification, consent apps... You can use commercial server such as Google, Azure, or any on-premise server.
- **Connectors**: inside Cells, they are pre-configured adapters to "talk" with external Identity Providers using the right protocol.

Both OIDC and SAML are relying on PKI to encrypt and certify exchanged data. If you are working inside a test environment without well-known-authority signed certificates, you may also prepare a local Certificate **Authority Server**.

## Data exchange workflow

[:image-popup:3_identity_management/cellsvsidps.png]

1. User opens Cells in a web browser. 
2. Cells renders login page to the user. If there are more than one Connectors, users are able to pick a connector to login. Cells basically ships an internal Connector providing a login dialog for cells internal users directory.
3. If the connector is external, user is redirected to the login page of Identity Server. User may use their credential (username/password), 2FA, etc. to authenticate to this server. If authentication succeeds, the Identity Server sends user identity information ()such as user id, email, groups, ...) in JWT or XML format to Cells and redirects the page back to Cells. This JWT/XML message will be introspected by Cells to allow or deny access.
4. If all this workflow succeeds, the user can access to their data (resource) inside Cells.

## Cells Connectors

Connectors can be added in Cells via the Cells Console > Authentication menu. Parameters will differ between each connector, but they can be summarized into four groups:

- **SSO URL** : URL of the Identity Server
- **Client ID/Secret** : Cells instance identification, must be previously registered inside the Identity Server
- **Redirect URL** : an endpoint which Cells open to Identity Server for sending back information. Usually looks like `https://cellssaml.pyd.io/auth/dex/callback`
- **Auxiliary infos** : Certificate Authority file, mapping from IdP attributes to Cells user attributes, etc.
  
In the Knowledge-base, you can find specific examples for the following Identity Providers :   
  
- [Google LINK TODO](/en/docs/kb/identity-management/using-google-identity-provider) (OIDC)
- [Github LINK TODO](/en/docs/kb/identity-management/using-github-identity-provider) (OIDC)
- [Azure ADFS LINK TODO](/en/docs/kb/identity-management/using-azure-adfs-identity-provider)
- [ADFS on-premise server LINK TODO](/en/docs/kb/identity-management/using-premise-adfs-server-identity-provider)
- [SimpleSAML LINK TODO](/en/docs/kb/identity-management/using-simplesaml-php-server-identity-provider) PHP server (SAML)
