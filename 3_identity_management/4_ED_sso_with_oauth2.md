# TODO

# Add a Github connector

## Create a Github Application

Create a New OAuth Application on **Github**,

[:image-popup:3_identity_management/sso_with_oauth2/github/github_create_app_1.png]

- **Application name:** Name your application
- **Homepage URL:** Your pydio Cells URL (main page)
- **Authorization callback URL:** http(s)://your-pydio**/auth/dex/callback** (the endpoint is the immutable part)

[:image-popup:3_identity_management/sso_with_oauth2/github/github_create_app_1.png]

[:image-popup:3_identity_management/sso_with_oauth2/github/github_create_app_3.png]

## Set the GitHub connector on Pydio Cells

In your Pydio Cells instance go to **Cells Console > Authentication > OAUTH2/OIDC > + Connector**.

[:image-popup:3_identity_management/sso_with_oauth2/github/cells_create_github_oidc_1.png]

[:image-popup:3_identity_management/sso_with_oauth2/github/cells_create_github_oidc_2.png]

Choose **GitHub**.

- **Client ID:** the client ID of your Github application (Fetched from github application, see step 1 )
- **Client Secret:** the client Secret of your Github application (Fetched from github application, see step 1)
- **Callback URL:** the same url defined during the creation of the GitHub application (Fetched from github application, see step 1)

[:image-popup:3_identity_management/sso_with_oauth2/github/cells_create_github_oidc_3.png]

# Add a Google Connector

## Create a Google Application for OIDC

### References

- https://cloud.google.com/identity-platform/docs/how-to-enable-application-for-oidc

### Step 1

Visit [https://console.cloud.google.com/](https://console.cloud.google.com/), 

- Go to **APIs & Services**

[:image-popup:3_identity_management/sso_with_oauth2/google/google_create_application_1.png]

- Navigate to **OAuth consent screen**

[:image-popup:3_identity_management/sso_with_oauth2/google/google_create_application_2.png]

And set the following fields:

- **Application name:** name your application
- **Authorized domains:** add your Pydio Cells instance domain
- **Application Homepage link:** put your Pydio Cells base url `https://my-cells.com`

- Hit **save**

[:image-popup:3_identity_management/sso_with_oauth2/google/google_create_application_3.png]

### Step 2

- Then go to **Credentials**

[:image-popup:3_identity_management/sso_with_oauth2/google/google_create_application_4.png]

- Click on **Create credentials**

[:image-popup:3_identity_management/sso_with_oauth2/google/google_create_application_5.png]

- Select **OAuth client ID**

[:image-popup:3_identity_management/sso_with_oauth2/google/google_create_application_6.png]

- Application Type : Select **Web Application**
- Press **Create**

[:image-popup:3_identity_management/sso_with_oauth2/google/google_create_application_7.png]

Last step, name your app (make sure to remember your **ID** and **Secret**) :

- **Authorised JavaScript origins:** Add your Pydio Cells url.
- **Authorised redirect URIs**: add a redirect url such as `https://my-cells.com/auth/dex/callback`, add at the end of your Pydio Cells URL **/auth/dex/callback** (this is the endpoint).
  
[:image-popup:3_identity_management/sso_with_oauth2/google/google_create_application_8.png]

[:image-popup:3_identity_management/sso_with_oauth2/google/google_create_application_9.png]

## Create a Google Connector in Cells

In your Pydio Cells instance go to **Cells Console > Authentication > OAUTH2/OIDC > + Connector**.

[:image-popup:3_identity_management/sso_with_oauth2/google/cells_google_oidc_create_0.png]

- Select **OpenID Connect**
- give it a label (name)

[:image-popup:3_identity_management/sso_with_oauth2/google/cells_google_oidc_create_1.png]

Then set the following parameters:

- **Canonical URL of the Provider:** `https://accounts.google.com`
- **Client ID:** your previously fetched client ID
- **Client Secret:** your previously fetched client Secret
- **Redirection URI:** the same URI that you have set during the google app creation.

[:image-popup:3_identity_management/sso_with_oauth2/google/cells_google_oidc_create_1.png]

- Hit save, return to your Pydio Cells login page, select google (depends on the label that you have set).