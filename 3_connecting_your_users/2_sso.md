## Single Sign On (SSO)

### What is it?

Single Sign-On is a service that allows a user to use one login session to access multiple applications.

### What does it mean in Pydio Cells?

Pydio Cells can be used as an [Identity Provider](./2_sso/1_cells_as_idp). In other words, any application can use the login session from Pydio Cells to validate access to a user and use its data.

### What does it mean in Pydio Cells Enterprise Distribution?

On top of that, Cells ED can integrate with multiple [External Identity Providers](./2_sso/2_ED_sso_with_idp). It automatically creates access for users from third-party services (commercial or on-premise) and understands multiple protocols (*OIDC*, *SAML2.0*, ...).

Furthermore, Cells ED provides a simple way to connect one or many external [LDAP or Active Directory](./2_sso/2_ED_binding_to_ldap) servers.

[:summary]

--------------------------------------------------------------------------------------------------------
_See Also_

- [OpenID Connect](https://openid.net/connect/)
- [OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)
