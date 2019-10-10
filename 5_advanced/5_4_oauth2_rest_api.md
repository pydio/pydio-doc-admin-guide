# Draft

With Pydio Cells 2 we are making use of the oauth2 authentication mechanism, enabling you to generate a token once and then proceed to make calls to the REST API.
In this guide, will be provided a list of the required values and examples for clients such as [Postman](https://www.getpostman.com/).


## Settings and values for OAUTH2 with Pydio Cells

|                  |                                                      |                                                             |
| ---------------- | ---------------------------------------------------- | ----------------------------------------------------------- |
| Auth_url         | http(s)://<pydio-cells>/oidc/oauth2/auth             |                                                             |
| Access token url | http(s)://<pydio-cells>/oidc/oauth2/token            |                                                             |
|                  |                                                      |                                                             |
| Client ID        | cells-sync                                           |                                                             |
| Client Secret    | no secret, this field can be empty                   |                                                             |
| Callback url     | http://localhost:3000/servers/callback               | can add redirect_url inside pydio.json (must restart cells) |
|                  |                                                      |                                                             |
| Scope            | openid email profile pydio offline                   |                                                             |
|                  |                                                      |                                                             |
| State            | you can write whatever you want 8 characters minimum |                                                             |



## Postman


Go to the **Authorization tab** and select the following values:

- Type: **OAuth 2.0**
- Add authorization data to: **Request Headers**

[:image-popup:5_advanced/postman_auth_type.png]

Then click on **Get New Access Token**.

[:image-popup:5_advanced/get_new_token.png]

A form will appear:

[:image-popup:5_advanced/postman_settings_oauth2.png]

(Assuming that a Pydio Cells instance is reachable with `http://192.168.0.110:8080`) of course for your case this part will have to be changed matching your Pydio Cells Instance.

| name                  | value                                       |     |
| --------------------- | ------------------------------------------- | --- |
| Token Name            | you can name it anything you want           |     |
| Grant Type            | Authorization Code                          |     |
| Callback URL          | http://localhost:3000/servers/callback      |     |
| Auth URL              | http://192.168.0.110:8080/oidc/oauth2/auth  |     |
| Access Token URL      | http://192.168.0.110:8080/oidc/oauth2/token |     |
| Client ID             | cells-sync                                  |     |
| Client Secret         | empty                                       |     |
| Scope                 | openid email profile pydio offline          |     |
| State                 | you can write anything                      |     |
| Client Authentication | Send as Basic Auth header                   |     |

Press **Request Token**, you will be asked to log with your Credentials (Pydio Cells user:password) and then redirected back to the application.

Now select the token by clicking its name.

[:image-popup:5_advanced/postman_select_token.png]

And you are good to go, you can query the API.

