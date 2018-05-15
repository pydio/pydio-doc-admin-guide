### Using the rest API

This document will guide you through a quick start with a REST API usig CURL


#### Retrieving an authentication token

Before performing any API call, one might get an authentication token. Here is the command to get an authencation:

``` bash
curl -X POST '<URL>/auth/dex/token' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=password&username=<USER>&password=<PASSWORD>&scope=email%20profile%20pydio&nonce=<NONCE>'
```

this returns a JSON response similar to this:

``` JSON
{
    "access_token":     "base64_content",
    "token_type":       "bearer",
    "expires_in":       "599",
    "refresh_token":    "base64_content",
    "id_token":         "base64_content"
}
```

with

- **URL**: The URL of the server
- **USER**: The name of the user to authenticate
- **PASSWORD**: The user password
- **NONCE**: a random string

#### Send request using the authentication token

Now we have a authentication token can do some expamples.

The rest API reference can be found int he admin Panel in the **ALL PLUGINS** section. And if we want to get the current authenticated user info, we can check what the API references tells us:

![REST API : user get info](./images/api_doc_user_service_get_info "Logo Title Text 1")
[!image-popup:]

So to get the information about the current authenticated user we can use this command:

``` Bash
curl -X GET \
  '<URL>/a/user/{{login}}' \
  -H 'Authorization: Bearer <TOKEN>' \
  -H 'Cache-Control: no-cache'
```
with

- **URL**: The url of your cells server
- **USER**: the user login
- **TOKEN**: The `id_token` field in the response of the authentication

