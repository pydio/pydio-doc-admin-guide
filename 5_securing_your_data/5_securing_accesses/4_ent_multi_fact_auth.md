Cells Enterprise Distribution provides a higher degree of security for users by enabling them to use Multi-Factor Authentication tools such as any TOTP compatible apps (like [Google Authenticator App](https://en.wikipedia.org/wiki/Google_Authenticator)), [Yubikey](https://www.yubico.com/) physical devices or [DUO Security](https://duo.com) application features.

| Yubikey                                                                        | Google Authenticator (or any TOTP compatible app)                                                 |  Duo Security Push                            |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------- |-----|
| [:image:5_securing_your_data/securing_user_access/multi_auth/yubikey_logo.png] | [:image-popup:5_securing_your_data/securing_user_access/multi_auth/google_authenticator_logo.png] | [:image-popup:5_securing_your_data/securing_user_access/multi_auth/duo-logo.png]| 


To enable Yubikey or TOTP, go to `Cells Console >> Application Parameters >> Authentication`.

[:image:5_securing_your_data/securing_user_access/multi_auth/enable_multi_auth_plugin.png]

Now each user has to go on its setting page that is located on the home page top left. User can choose the preferred mean of authentication and follow the instructions below for further details on each of them.

[:image:5_securing_your_data/securing_user_access/multi_auth/user_settings_multi_auth.png]

See below for instructions about enabling DUO Security.

## TOTP Apps (Google Authenticator, Free OTP, etc)

[:image:5_securing_your_data/securing_user_access/multi_auth/google_auth_config.png]

To enable the Google Authenticator:

1. Scan the QRCode with your app
2. Put the generated code in the **Google Authenticator Code** field.
3. Click on test, to verify that it's functioning.
4. Save.

*__Do not forget to note the recovery code, if you ever loose your authenticator(smartphone or could be a reinstall/data loss) it will enable you to log back in and set a new one or remove it.__*

## Yubikey

[:image:5_securing_your_data/securing_user_access/multi_auth/yubikey_config.png]

To use your Yubikey you first need to retrieve the clientID and API Secret, to do so follow this link
[https://upgrade.yubico.com/getapikey/](https://upgrade.yubico.com/getapikey/) put your email address and the Yubikey OTP that you will find in your package.

*As told on the Yubikey site wait up to 5 mins before starting to set your key up*.

Plug your Yubikey to your computer, install and follows the steps: depending on your OS, this is usually plug & play.

Now:

1. Pur your CliendID and API Secret
2. Put the focus on the **Test your Key** field and press once on your Yubikey
3. Click on test, to verify that the information are correct
4. Save

## DUO Security

This assumes you have an Admin Account already set up on the DUO website. Logged in on DUO, use "Protect An Application > Auth API" to create API Keys.

Enable DUO Security plugin in the Authentication > Plugins screen.

[:image:5_securing_your_data/securing_user_access/multi_auth/duo-enable-plugin.png]

Then edit the plugin options by providing the following values : 

 - API Key, API Secret as provided by DUO
 - API Endpoint as provided by DUO
 - Application identifier : **create your own** random 40 characters string to identify this installation.

Select the "Active Dual-form Authentication" checkbox to enable this for all users. This parameter can be refined on a group/role/user base to have it active only for a subset of users.

[:image:5_securing_your_data/securing_user_access/multi_auth/duo-plugin-settings.png]

Once saved, at next login, users will be prompted with a specific configuration window to setup their DUO mobile application.
