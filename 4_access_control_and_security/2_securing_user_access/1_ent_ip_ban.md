By default the ip ban feature is enabled to protect the pydio cells instance from brute force attacks such a DDOS attack.

It's also protecting users from having someone on the exterior trying to guess their password.

### Ip ban service

All the current bans are displayed on this interface you can choose perform an action on the bans listed such as
unbanning or blacklisting a banned ip, there will be further details about that below.

[:image-popup:4_access_control_and_security/securing_user_access/ip_ban_interface.png]

### White and Black list

You can whitelist an ip meaning that this ip will have access to cells no matter what or you can blacklist an ip and then it will have no more access to the cells instance.

[:image-popup:4_access_control_and_security/securing_user_access/ip_ban_black_white_list.png]

### Configure how the ban occurs

The default behavior is a ban will occur on the ip that is trying to connect more than 10 times under a 5 seconds time period the ban will expire after 10 minutes.

You can change those values by going to **All PLugins > IPBan Service**.

[:image-popup:4_access_control_and_security/securing_user_access/ip_ban_plugin.png]
