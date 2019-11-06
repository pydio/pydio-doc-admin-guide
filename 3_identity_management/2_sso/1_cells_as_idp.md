An Identity provider is a trusted provider that lets you use Single Sign-On (SSO) to access other websites.

## OpenID Connect

OpenID Connect 1.0 is a type of Identity Provider working as a simple identity layer on top of the OAuth 2.0 protocol. It allows Clients to verify the identity of the End-User based on the authentication performed by an Authorization Server, as well as to obtain basic profile information about the End-User in an interoperable and REST-like manner. You can read more about it [here](https://openid.net/connect/).

Cells provides OIDC endpoints, making it an Identity Provider for external applications. The standard Cells client applications (web frontend, sync, cells-client) do already use this protocol to handle authentication to Cells. As OpenIDConnect is a layer built on top of OAuth2.0, any OAuth2-enabled application will be able to use Cells as its Identity Provider.

## Involved Parties

When configuring Cells as an Identity provider, please keep in mind that you are working in an environment where serveral entities must know each other and exchange security information via public network. You must know the role of each entity and the protocol required to exchange information in a specific order. 

- **Resource Owner**: the owner of the data your application requires access to, (usually the user who is connecting)
- **Authorization Server**: the server delivering tokens to the client. (Cells)
- **Resource Server** : The application that holds the resource the client wants to access. (Cells - holds the user profile information and access to the user files)
- **Clients**: application that need access to resources on behalf of its owner. Must first be registered with the Authorization Server.

## OIDC/OAuth2 Endpoints

| Endpoint           | URL                                                                |
| -----------------  | ------------------------------------------------------------------ |
| OIDC Configuration | http(s)://your-cells.com**/oidc/.well-known/openid-configuration** |
| Authorization      | http(s)://your-cells.com**/oidc/oauth2/auth**                      |
| Token              | http(s)://your-cells.com**/oidc/oauth2/token**                     |

## Registering an OIDC Client 

[:image:3_identity_management/2_sso/create_client.gif]

## Examples

See the API docs