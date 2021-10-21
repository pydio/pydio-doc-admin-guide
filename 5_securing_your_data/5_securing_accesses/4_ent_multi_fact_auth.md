
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v2. Looking for <a href="https://pydio.com/en/docs/cells/v3/quick-start">Pydio Cells v3 docs?</a></span>
</div>




### Multi Factor Authentication

| Yubikey                                                                        | Google Authenticator                                                                              |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------- |
| [:image:5_securing_your_data/securing_user_access/multi_auth/yubikey_logo.png] | [:image-popup:5_securing_your_data/securing_user_access/multi_auth/google_authenticator_logo.png] |

With Cells Enterprise Distribution, we offer a higher degree of security for users by enabling them to use authenticators such as the [Google Authenticator App](https://en.wikipedia.org/wiki/Google_Authenticator) or a [Yubikey](https://www.yubico.com/).

To enable this feature, go to `Cells Console >> Application Parameters >> Authentication`.

[:image:5_securing_your_data/securing_user_access/multi_auth/enable_multi_auth_plugin.png]

Now each user has to go on its setting page that is located on the home page top left (the 3 dots).

[:image:5_securing_your_data/securing_user_access/multi_auth/user_settings_multi_auth.png]

Choose the preferred mean of authentication and follow the instructions below for further details on each of them.

### Google Authenticator

[:image:5_securing_your_data/securing_user_access/multi_auth/google_auth_config.png]

To enable the google authenticator:

1. Scan the QRCode with your app
2. Put the generated code in the **Google Authenticator Code** field.
3. Click on test, to verify that it's functioning.
4. Save.

*__Do not forget to note the recovery code, if you ever loose your authenticator(smartphone or could be a reinstall/data loss) it will enable you to log back in and set a new one or remove it.__*

### Yubikey

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

### Troubleshooting

If a user has lost any access due to loosing the authenticator you can disable the setting by making an API call to the server.
