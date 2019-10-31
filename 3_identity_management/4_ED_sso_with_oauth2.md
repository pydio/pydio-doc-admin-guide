# TODO

# Add a Github connector

## Create a Github Application

Create a New OAuth Application on **Github**,

[:image-popup:3_identity_management/github/github_create_app_1.png]

- **Application name:** Name your application
- **Homepage URL:** Your pydio Cells URL (main page)
- **Authorization callback URL:** http(s)://your-pydio**/auth/dex/callback** (the endpoit is the fixed part)

[:image-popup:3_identity_management/github/github_create_app_2.png]

[:image-popup:3_identity_management/github/github_create_app_3.png]

## Set the GitHub connector on Pydio Cells

In your Pydio Cells instance go to **Cells Console > Authentication > OAUTH2/OIDC > + Connector**.

<img src="/Users/jay/Desktop/Screenshot 2019-10-30 at 16.58.27.png" style="zoom:50%;" />

[:image-popup:3_identity_management/github/cells_create_github_oidc_1.png]

<img src="/Users/jay/Desktop/Screenshot 2019-10-30 at 16.50.17.png" alt="Screenshot 2019-10-30 at 16.50.17" style="zoom:50%;" />

[:image-popup:3_identity_management/github/cells_create_github_oidc_2.png]

Choose **GitHub**.

- **Client ID:** the client ID of your Github application (see screenshot above)
- **Client Secret:** the client Secret of your Github application (see screenshot above)
- **Callback URL:** the same url defined during the creation of the GitHub application (see screnshot above)

[:image-popup:3_identity_management/github/cells_create_github_oidc_3.png]

# Add a Google Connector

## Create a Google Application for OIDC

### References

- https://cloud.google.com/identity-platform/docs/how-to-enable-application-for-oidc

### 1)

Visit [https://console.cloud.google.com/](https://console.cloud.google.com/), 

- Go to **APIs & Services**

<img src="/Users/jay/Desktop/Screenshot 2019-10-31 at 10.10.36.png" style="zoom:33%;" />

[:image-popup:3_identity_management/google/google_create_application_1.png]

- First go to **OAuth consent screen**

<img src="/Users/jay/Desktop/Screenshot 2019-10-31 at 10.38.06.png" style="zoom:33%;" />

[:image-popup:3_identity_management/google/google_create_application_2.png]

And set the following fields:

- **Application name:** name your application
- **Authorized domains:** add your Pydio Cells instance domain
- **Application Homepage link:** put your Pydio Cells base url (https://my-cells.com)

- Hit **save**

![](/Users/jay/Desktop/Screenshot 2019-10-31 at 10.14.51.png)

[:image-popup:3_identity_management/google/google_create_application_3.png]

### 2) 

- Then go to **Credentials**

<img src="/Users/jay/Desktop/Screenshot 2019-10-31 at 10.11.35.png" style="zoom:33%;" />

[:image-popup:3_identity_management/google/google_create_application_4.png]

- Click on **Create credentials**

<img src="/Users/jay/Desktop/Screenshot 2019-10-31 at 10.12.04.png" style="zoom:33%;" />

[:image-popup:3_identity_management/google/google_create_application_5.png]

- Select **OAuth client ID**

<img src="/Users/jay/Desktop/Screenshot 2019-10-31 at 10.12.13.png" style="zoom: 25%;" />

[:image-popup:3_identity_management/google/google_create_application_6.png]

- Application Type : Select **Web Application**
- Press **Create**

<img src="/Users/jay/Desktop/Screenshot 2019-10-31 at 10.12.39.png" style="zoom:33%;" />

[:image-popup:3_identity_management/google/google_create_application_7.png]

Last step, now name your app (make sure to save your **ID** and **Secret**) :

- **Authorised JavaScript origins:** Add your Pydio Cells url.
- **Authorised redirect URIs**: add a redirect url such as `https://my-cells.com/auth/dex/callback`, the basically add at the end of your Pydio Cells URL **/auth/dex/callback** (this is the endpoint).
- 
<img src="/Users/jay/Desktop/Screenshot 2019-10-31 at 10.46.46.png" style="zoom:33%;" />

[:image-popup:3_identity_management/google/google_create_application_8.png]

[:image-popup:3_identity_management/google/google_create_application_9.png]


## Create a Google Connector in Cells

In your Pydio Cells instance go to **Cells Console > Authentication > OAUTH2/OIDC > + Connector**.

<img src="/Users/jay/Desktop/Screenshot 2019-10-30 at 16.58.27.png" style="zoom:50%;" />

[:image-popup:3_identity_management/google/cells_google_oidc_create_0.png]

- Select **OpenID Connect**
- give it a label (name)

<img src="/Users/jay/Desktop/Screenshot 2019-10-31 at 10.52.01.png" style="zoom:33%;" />

[:image-popup:3_identity_management/google/cells_google_oidc_create_1.png]

Then set the following parameters:

- **Canonical URL of the Provider:** `https://accounts.google.com`
- **Client ID:** your previously fetched client ID
- **Client Secret:** your previously fetched client Secret
- **Redirection URI:** the same URI that you have set during the google app creation.

[:image-popup:3_identity_management/google/cells_google_oidc_create_1.png]


- Hit save, return to your Pydio Cells login page, select google (depends on the label that you have set).

  