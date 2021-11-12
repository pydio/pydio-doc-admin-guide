<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP). Time to move to <a href="https://pydio.com/en/docs/administration-guides">Pydio Cells</a>!</span>
</div>

Pydio comes with a bundled webDAV **server** which allows you to expose your files to a variety of clients applications for easy access. All you need to do is to "Allow webDAV Access" through Pydio's web interface.

### WebDAV feature requirements & activation
If you managed to properly configure the Pydio rewrite rule everything should work smoothly.

#### _WebDAV URLs in Pydio_

To understand the configurations, lets explain how this will work : your Pydio is installed on a server **_http[s]://yourdomain.com_**, inside the folder **_pydio_** for example. Normal web access is then **http[s]://yourdomain/pydio/** (or **http[s]://yourdomain/**). What we’ll do is create a “virtual” folder that will be the root of all DAV urls. By default, this is called “shares”, but you can change it. On this basis, any workspace will be accessible (by an accredited user of course) at the following url : **http[s]://yourdomain.com/pydio/shares/workspace_id or http[s]://yourdomain.com/pydio/shares/workspace_alias** (Or in this case : **http[s]://yourdomain.com/shares/workspace_id or http[s]://yourdomain.com/shares/workspace_alias**).

When you want to share you can use the base url **http[s]://yourdomain.com/pydio/shares/** (or **http[s]://yourdomain.com/shares/**) that will also be accessible, displaying all the **logged user** workspaces like folders. Thus it is generally the unique URL you have to provide to your users.

#### _Pydio configuration_

The WebDAV configurations can be changed through the web user interface. You’ll find them in the Settings section, under Application Parameters > Application Core, there is a “WebDAV Server” header.

[:image-popup:6_advanced_configuration/webdav_configuration_update.png]

Options are described below:

+ **Enable WebDAV** : totally enable or disable the webdav feature. false by default.
+ **Enable for all users** : you can now enable webDAV for all users, they no longer need to take action.
+ **Shares URI** : the exact mirror of the previous .htaccess configs : path to the virtual directory, thus something like “/files/shares”. Make sure to start with a slash, and end without slash.
+ **Shares Host** : the Host used in your webdav protocol.
+ **Digest Realm** : used for the digest authentication, and to store the webdav password encoded. Thus, if you want to change this, change it the very first time you install the feature, otherwise you’ll have to ask the users to re-enter their password.
+ **Force Basic Auth** : in some case, you know that the clients you will be using can only use Basic Authentication, and you want to force this authentication method. It is best secured to leave it to No.
+ **Browser Access** : wether the shares URL are directly browsable in WebDAV through the browser. This generally creates a kind of duplicate of the standard web-based interface, but it is generally VERY useful to enable that one to test if the configuration is correct.

### Per-user activation and authentication
Once the webDAV is globally activated, each user will still have to manually activate it for his/her account.

In some cases, the user will have to create a dedicated password for webdav authentication. This is not very handy, but unfortunately it can be necessary if the TRANSMIT_CLEAR_PASS option of the Pydio Authentication is set to No. The webDAV authentication mechanism implies either knowing the password in its clear form (basic) or in its encoded form (digest), in the latter case using a different algorithm than the one used by Pydio. For this reason, we have to store the user webDAV password in an encrypted form as a user preference, and he/she has to enter this password manually the first time the webDAV is activated.

In both case, the WebDAV activation (and password configuration) is under the user menu, in WebDAV preferences.

[:image-popup:6_advanced_configuration/webdav_preferences_update.png]

### Known issues, limitations, workarounds

+ **Authorization mechanism** may fail in the Apache + PHP as CGI case, and some more lines need to be added to the .htaccess file to ensure correct headers redirection. See the .htaccess file for more info, simply uncomment the lines.

+ There are plenty of documentation about WebDAV, both client and server on the web, one very useful website is the wiki of the **SabreDAV** project. It is the library we use, and most well-kown webDAV clients are carefully documented. See for example http://sabre.io/dav/ for the previous problem.
