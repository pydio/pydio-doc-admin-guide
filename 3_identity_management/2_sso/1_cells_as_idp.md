As explained in the SSO introduction, Cells can act as both a Resource Provider and an Identity Provider with third party services. This section describes the latter case.

## OpenID Connect

OpenID Connect 1.0 is a simple identity layer on top of the OAuth 2.0 protocol. It allows Clients to verify the identity of the End-User based on the authentication performed by an Authorization Server, as well as to obtain basic profile information about the End-User in an interoperable and REST-like manner. You can read more about it [here](https://openid.net/connect/).

Cells provides OIDC endpoints to be used as an Identity Provider for any of your external applications. The standard Cells client applications (web frontend, sync, cells-client) do already use this protocol to handle authentication to Cells. As OpenIDConnect is a layer built on top of OAuth2.0, any OAuth2-enabled application will be able to use Cells as its Identity Provider.

## Involved Parties

When configuring Cells with external providers, please keep in mind that you are working in an environment where serveral entities must know each other and exchange security information via public network. You must know the role of each entity and the protocol required to exchange information in a specific order. 

- **Users**: the owner of your application data (_resource_), whose identification information is owned by the Cells. 
- **Cells**: the _Identity Provider_ in OIDC
- **Resource Server** : The application that wants to use Cells as an Identity Provider.
- **Clients**: inside Cells, identification information for each Resource Server. Must always be registered as a first step to initiate the communication between Cells and the external application.

## OIDC/OAuth2 Endpoints

TODO 

## Configuring an OIDC Client 

TODO

## Examples

See the API docs