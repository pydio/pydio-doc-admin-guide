In this tutorial we will show you how to configure the servers to active Kerbers SSO in pydio Cells.
We will work on three servers/machines.

- Windows Server - Domain Controller
- Cells Server: a linux server running cells - web service server
- Workstation + Web browser (Firefox, Chrome, Edge)

## Windows Server
###Create a service account
Create a new user “cellssrv” in Users organizational unit.

[:image:3_connecting_your_users/2_sso/image6.png]

Make sure that this user can’t change the password as well as the password never expires.
[:image:3_connecting_your_users/2_sso/image7.png]

Two other option are required to be checked is 
Kerberos AES 235 bits
Do not require Kerberos preauthentication

[:image:3_connecting_your_users/2_sso/image8.png]

Generate the keytab
Open PowerShell and execute following command to convert the “cellssrv” to service account

The command should be written in one line.

```
PS C:\Users\Administrator> ktpass /princ HTTP/cellssrv.lab.py@LAB.PY  /pass "Passw0rd" /mapuser cellssrv /out cellssrv.keytab
/ptype KRB5_NT_PRINCIPAL  /crypto AES256-SHA1 /mapop set
```


>Attention: 
lab.py is the domain name and it should be in lowercase
@LAB.PY is the domain name and it should be in uppercase
you should replace by your domain name

[:image:3_connecting_your_users/2_sso/image3.png]

After the command, you can see the user logon is changed to “HTTP/cellssrv.lab.py”
Then copy the "cellssrv.keytab" to Cells’ server e.g: `/var/cells/cellssrv.keytab`


[:image:3_connecting_your_users/2_sso/image4.png]
DNS record
Make sure that your dns server has a A record for cellssrv machine. Otherwise, crease a new Host record.
[:image:3_connecting_your_users/2_sso/image5.png]

>Attention: Your server cells is running at https://cellssrv.lab.py. If it’s running at https://other-name.lab.py, please create an “other-name” account service.

## Cells Server
### Key tab
Copy the generated keytab (cellssrv.keytab) to the Cells server. Change the permission so that “pydio” user can read. For example “/var/cells/cellssrv.keytab”
Env variables
Add following lines to systemd file config (/etc/systemd/system/cells.service)

```
…
[Service]
WorkingDirectory=/home/pydio
PermissionsStartOnly=true

AmbientCapabilities=CAP_NET_BIND_SERVICE
ExecStart=/home/pydio/cells-enterprise start
Restart=on-failure
StandardOutput=journal
StandardError=inherit
LimitNOFILE=65000
TimeoutStopSec=5
KillSignal=INT
SendSIGKILL=yes
SuccessExitStatus=0

# Add environment variables

Environment=CELLS_ENABLE_METRICS=false
Environment=CELLS_WORKING_DIR=/var/cells

Environment=CELLS_SPNEGO_LABEL="Kerberos SSO"
Environment=CELLS_SPNEGO_KEYTAB="/var/cells/cellssrv.keytab"
….
```

**CELLS_SPNEGO_LABEL**: is the label of the button on the login screen on which user will login with SSO Kerberos when they click
**CELLS_SPNEGO_KEYTAB**: the absolute path of keytab

>Attention: Don’t forget to **systemctl daemon-reload** then **systemctl restart cells**

### Users' PC & Web browser
At this step, you can see the “Kerberos SSO” button on the login page of Cells. But it does not work because the authentication negotiation has not been activated.
#### Enable spnego 
In a joined-domain PC, open Internet Options then add “https://cellssrv.lab.py” to the local internet zone

[:image:3_connecting_your_users/2_sso/image2.png]

[:image:3_connecting_your_users/2_sso/image1.png]

After adding this option, you are able to authenticate with “Kerberos SSO”. When you click on this button, you are logged in with the current window user.

#### The configuration in “Internet Option” works with Chrome, Edge
If you are using Firefox, please visit this link to enable spnego: https://www.ibm.com/docs/en/was/9.0.5?topic=authentication-configuring-client-browser-use-spnego

#### GroupPolicy
You are also able to enable authentication negotiation (spnego) for web browsers via Domain Controller Group Policy

##### Google Chrome: 
*Document*: https://docs.keeper.io/enterprise-guide/deploying-keeper-to-end-users/keeper-fill/group-policy-deployment-chrome#:~:text=your%20Chrome%20Policy-,1.,and%20create%20a%20New%20Policy.
*Chrome gpo template*: https://chromeenterprise.google/browser/download/#manage-policies-tab

##### Firefox 
*Document*: https://specopssoft.com/blog/using-firefox-enterprise-gpos-enable-windows-integrated-authentication-specops-websites/
*Firefox gpo template*: https://github.com/mozilla/policy-templates/releases