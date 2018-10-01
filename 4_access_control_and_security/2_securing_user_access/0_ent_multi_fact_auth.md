### Multi Factor Authentication

[:image-popup:4_access_control_and_security/securing_user_access/yubikey.png] [:image-popup:4_access_control_and_security/securing_user_access/google_authenticator.png]

With the enterprise version we offer a higher degree of security for users by enabling them to use authenticators such as the [Google Authenticator App](https://en.wikipedia.org/wiki/Google_Authenticator) or a [Yubikey](https://www.yubico.com/).

To enable this feature, on the left menu in the **Appliation Parameters** section go to **Authentication**.

*To display the **Authentication** menu you have to be on advanced mode, to do so enable the slider that is located on the top right as seen on this screenshot*

[:image-popup:4_access_control_and_security/securing_user_access/advanced_menu.png]

[:image-popup:4_access_control_and_security/securing_user_access/enable_multi_auth_plugin.png]

Now each user has to go on it's setting page that is located on the home page top left (the 3 dots).

[:image-popup:4_access_control_and_security/securing_user_access/user_settings.png]

[:image-popup:4_access_control_and_security/securing_user_access/user_multi_auth.png]

Choose the prefered mean of authentication and follow the instructions below for further details on each of them.

### Google Authenticator

[:image-popup:4_access_control_and_security/securing_user_access/google_auth_config.png]

To enable the google authenticator:

1. Scan the qrcode with your app
2. Put the generated code in the **Enroll.Google.TestCode.Field**
3. Click on test, to verify that it's functionning.
4. Save.

*Do not forget to note the recovery code, if you ever loose your authenticator(smartphone or could be a reinstall/data loss) it will enable you to log back in and set a new one or remove it.*

### Yubikey

[:image-popup:4_access_control_and_security/securing_user_access/yubikey_config.png]

To use your Yubikey you first need to retrieve the clientID and API Secret, to do so follow this link
[https://upgrade.yubico.com/getapikey/](https://upgrade.yubico.com/getapikey/) put your email address and the Yubikey OTP that you will find in your package.

*As they told on the Yubikey site wait up to 5 mins before starting to set your key up*

Plug your Yubikey to your computer install and follows the steps.(should be plug & play)

Now:

1. Pur your CliendID and API Secret.
2. Put the focus on the **Test your Key** field and press once on your Yubikey.
3. Click on test, to verify that the informations are correct.
4. You can now save.