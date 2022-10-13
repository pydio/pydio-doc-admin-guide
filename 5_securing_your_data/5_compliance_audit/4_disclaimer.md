
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v3. Looking for <a href="https://pydio.com/en/docs/cells/v4/quick-start">Pydio Cells v4 docs?</a></span>
</div>

Disclaimer features provides a way to force your users to accept your Terms of Use before being allowed to use the platform. This is typically suited to fit compliance rules from your company or GDPR european laws.

### Enable the disclaimer

You first need to enable the plugin, to do that browse to the following menu **Cells Console >> Identity Management >> Authentication**.

You are now presented with the following settings window, you need to enabled it and then you can edit it to add your custom message.

[:image:5_securing_your_data/auditing_accesses/disclaimer/disclaimer_enable.png]

**HTML** elements are also working in the disclaimer text field as you can notice on the screenshot below (*see the user authentication screenshot for the end result*).

You now also have an option to display a disclaimer for the public links.

[:image:5_securing_your_data/auditing_accesses/disclaimer/disclaimer_settings.png]

### User authentication

When a user authenticates for the first time he will be greeted with the following message and will have to agree and validate before being able to access the application.

> This will also happen for password protected public links if the option is **enabled**.

[:image:5_securing_your_data/auditing_accesses/disclaimer/disclaimer_user_login.png]