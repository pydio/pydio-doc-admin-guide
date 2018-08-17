
This document guides you through a quick start with a REST API using CURL.

## Requirements

The API_KEY and API_SECRET are required to perform any request. They can be found in "**Configs Backend**" in the admin settings.

### Retrieving an authentication token

In addition of the API and SECRET, most of the request require an authentication token. 

To get an authentication token via the command line, execute:

```sh
curl -X POST '<URL>/auth/dex/token' \
-u <API_KEY>:<APISECRET> \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=password&username=<USER>&password=<PASSWORD>&scope=email%20profile%20pydio&nonce=<NONCE>'
```

where:

- **API_KEY**: The API access Key generated at the install
- **API_SECRET**: The API secret also generated at the install
- **URL**: The URL of the server
- **USER**: The name of the user to authenticate
- **PASSWORD**: The user password
- **NONCE**: a random string

It returns a JSON response similar to this:

```JSON
{
    "access_token":     "base64_content",
    "token_type":       "bearer",
    "expires_in":       "599",
    "refresh_token":    "base64_content",
    "id_token":         "base64_content"
}
```

### Send request using the authentication token

We now have an authentication token, let us make some example calls.

The REST API reference can be found in the admin Panel in the **ALL PLUGINS** section. So, let's say that we want to get the current authenticated user info, we can check what the API references tells us:

[:image-popup:5_advanced/rest_user_service_get_ref.png]

To get the information about the current authenticated user we can use this command:

``` Bash
curl -X GET \
  '<URL>/user/{{login}}' \  
  -u <API_KEY>:<APISECRET> \
  -H 'Authorization: Bearer <TOKEN>' \
  -H 'Cache-Control: no-cache'
```

with

- **API_KEY**: The API access Key generated at the install
- **API_SECRET**: The API secret also generated at the install
- **URL**: The url of your Pydio Cells server
- **USER**: the user login
- **TOKEN**: The `id_token` field in the response of the authentication
