[Draft]
# Introduction

Single Sign On (SSO) is the most important part of a cloud-oriented application like Pydio Cells. It allows the developers to separate the authentication from the logic of application, then freely to choose a third-party service for this process. This model is great advantage to users to reuse their credential on a unique login page to access to other web applications. That's why we developed in Cells version 2.0 the capability to connect to Identity Providers such as Google OIDC, Azure ADFS... This how-to will introduce to you how Cells interact with Identity Providers in general. We will go futher in detail through the examples.

Before going into this how-to, it's important to understand the following concepts:

- OpenID Connect 2.0 (https://openid.net/connect/)
- OAuth 2.0 Authorization Framework (https://tools.ietf.org/html/rfc6749)
- Security Assertion Markup Language (SAML) (http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0-cd-02.html)

# Overview

When you configure Cells with external connectors, please keep in mind that you are working in an environment where serveral entities known each other and exchange security information via public network. You must know the role of each entity and the protocol that define the process of exchanging information in a specific order. Although OIDC and SAML are different frameworks, but they are the games of following parties:
- Users: the owner of data (resource) located at Cells, but the identifing information is located at Identity Provider Server. Before accessing to their resources in Cells, users must identify at Identity Provider Server then authorize Cells to access to resource on behaft.
- User Agent: the client application/device which users use to connect to Cells. It's can be web browser, sync client application or mobile application.
- Cells (Resource Server in OIDC, or relaying party in SAML)
- Identity Provider Server: Depending on framework, the server provides several services such as, authentication, identification, manage consent apps... You can use commercial server such as google, azure, or your on-premise server.

Both of OIDC and SAML are rely on PKI to encrypt, certify exchanged data. If you are working in test environment without well-known-authority signed certificates, you may prepare a local Certificate Authority Server.

## Exchange of security information between cells and other entities

[:image-popup:3_identity_management/connectors/cellsvsidps.png]

1. User browses Cells on a web browser. 
2. Cells renders login page and sends back to user. If there are more than one configured connectors, users are able to select connector to login.
3. If user select external connector, the page will redirect to login page of Identity Server. Depending on Server, user may use credential (username/password), 2FA, to authenticate to the server. If user is authenticated, Identity Server sends user identity information such as user id, email, groups, ...in JWT or XML format to Cells and redirect the page back to cells url. If Cells receive a JWT/XML message from Identity Server, it will examine, verify and decide to allow or deny access.
4. If all things is positive, user can enter to cells to access to their data (resource).


## Configuration in Cells
A new connector can be added into Cells via admin console or directly in pydio.json. The parameters are different between contectors but they can be classified into 04 groups:
- The SSO URL of Identity Server
- Client ID / Secret for Cells registed in Identity Server
- The redirect URL - the endpoint which Cells open for request from Identity Server. Usually is "https://cellssaml.pyd.io/auth/dex/callback"
- Auxiliary infos such as CA file, user attribute names ...
  
# Examples
- Pydio Cells with Azure ADFS
- Pydio Cells with ADFS on-premise server
- Pydio Cells with SimpleSAMLphp server
  
## Azure ADFS (via OpenID Connect)
### Register cells application in Azure
Please visit [this article](https://docs.microsoft.com/en-us/graph/auth-register-app-v2) to register new application on azure.
- The redirect URI (or reply URL) for cells application is always in format: https://server.cells.domain/auth/dex/callback
- You should create a new "client secret" in "Certificates and secrets" of new registed application
  [:image-popup:3_identity_management/connectors/connector_azure_08.png]

When finished, takes a note on following information:
- Directory (tenent) ID
- Client Secret
- The OpenID Connect metadata document ([document link](https://login.microsoftonline.com/tenentId/v2.0)). 
  
  In Cells, the metadata url is automatically added ".well-known/openid-confuguration" at the end. It looks like: https://login.micosoftonline.com/[tenant_id]/v2.0
   [:image-popup:3_identity_management/connectors/connector_azure_07.png]  

## Add new connector in Cells
When you finished the registration new app in Azure, go to the admin console of Cells to add a new connector: type "Microsoft"
  [:image-popup:3_identity_management/connectors/connector_azure_01.png]

  [:image-popup:3_identity_management/connectors/connector_azure_01.png]

# On-premise ADFS (SAML)
## Install and configure ADFS service on windows server 2012
This how-to assumes that you have had already a domain and ADFS server. If you haven't installed ADFS server yet, please visit [this link](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/windows-server-2012-ad-fs-deployment-guide) for more information. In our test environment, we have a windows server 2012 running in "win.pyd.io" domain. After installation new AD FS role, the service has SSO URL is: https://win.pyd.io/adfs/ls/

## Add new Relaying Party Trust
This [link](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/create-a-relying-party-trust) will introduce by steps the how-to register new application to AD FS server. What we do in this section is telling AD FS our server information such as name, callback url, SAML Binding method...

Registered cells application in Relaying Party Trusts

[:image-popup:3_identity_management/connectors/connector_onpremiseadfs3.png]

Set endpoints of Cells application in Relaying Party Trusts

[:image-popup:3_identity_management/connectors/connector_onpremiseadfs4.png]

## Add new connector in Cells

[:image-popup:3_identity_management/connectors/connector_onpremiseadfs1.png]

1. *pydio-saml-onpremise-adfs* is connector identity. It's unique string in the list of connector in Cells.
2. *On-Premise ADFS SAML* the string on login page of Cells to allow user to select connector to authenticate
3. *https://win.pyd.io/adfs/ls/* the url of adfs server that handle the authentication.
4. The path to certificate file which is used by ADFS server to sign saml response
5. The callback url in Cells is always in this format: "https://domain.com/auth/dex/callback
6. The name of attribute in adfs saml response is longer than normal. They are usually:
   1. http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
   2. http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
7. 

[:image-popup:3_identity_management/connectors/connector_onpremiseadfs2.png]


# SimpleSAMLphp (idp)
This is an example of connecting Pydio Cells with an open-source saml server. SimpleSAMLphp is written in native PHP that deals with authentication. For more information, please visit [this link](https://simplesamlphp.org/)

## Install and configure SimpleSAMLphp server

It's possible to configure SimpleSAMLPhp to serve as a Service Provider or as Idetity Provider. In this how-to, we are going to setup a Identity Provider which provider services to Cells - a service provider.

Following the docs of simplesamlphp, you can easily download source code and setup a web server on a linux box. For demonstration only, a simple authsource is set in simplesamlphp by copying the sample accompanied source code to config/authsources.php. 
For further information, please visit [this link](https://simplesamlphp.org/docs/stable/simplesamlphp-idp)

```php
<?php
$config = [
    // This is a authentication source which handles admin authentication.
    'admin' => [
        // The default is to use core:AdminPassword, but it can be replaced with
        // any authentication source.
        'core:AdminPassword',
    ],
    'example-userpass' => [
        'exampleauth:UserPass',
        'student:studentpass' => [
            'uid' => ['student'],
            'email' => ['stu01@lab.py'],
            'eduPersonAffiliation' => ['member', 'student'],
        ],
        'employee:employeepass' => [
            'uid' => ['employee'],
            'email' => ['emp001@lab.py'],
            'eduPersonAffiliation' => ['member', 'employee'],
        ],
    ],
];

```

Enable Idp protocol in config/config.php

```php
    'enable.saml20-idp' => true,
    'enable.shib13-idp' => false,
    'enable.adfs-idp' => false,
    'enable.wsfed-sp' => false,
    'enable.authmemcookie' => false,

```

Configure certificate/key for idp in metadata/saml20-idp-hosted.php

```php
    // X.509 key and certificate. Relative to the cert directory.
    'privatekey' => 'key.pem',
    'certificate' => 'cert.pem',
```

Add Cells - Service Provider into metadata/saml20-sp-remote.php
```php
/*
 * Example SimpleSAMLphp SAML 2.0 SP
 */
$metadata['https://cells.lab.py/auth/dex/callback'] = [
    'AssertionConsumerService' => 'https://cells.lab.py/auth/dex/callback',
    'SingleLogoutService' => 'https://cells.lab.py/auth/dex/logout',
];
```

## Add new connector in Cells
You've done the configuration of a saml server and registered cells with id "https://cells.lab.py/auth/dex/callback". The saml server are able to accept the request comming from you Cells and return back the result of authentication to callback url.

[:image-popup:3_identity_management/connectors/connector_simplesaml_01.png]
