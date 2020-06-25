By default the ip ban feature is enabled to protect the Pydio Cells instance from brute force attacks on passwords.
It's also protecting users from having someone trying to guess their password.

_The following features are only available on our Enterprise Edition_

### IP ban service

All the current bans are displayed on this interface you can choose perform an action on the bans listed such as
unbanning or blacklisting a banned ip, there will be further details about that below.

[:image:5_securing_your_data/securing_user_access/ip_ban_service/ip_ban_service_interface.png]

### Whitelist and Blacklist

You can whitelist an ip meaning that this ip will have access to cells no matter what or you can blacklist an ip and then it will have no more access to the cells instance.

### Configure how the ban occurs

The default behavior is a ban will occur on the ip that is trying to connect more than 10 times under a 5 seconds time period the ban will expire after 10 minutes.

You can change those values by going to **All PLugins > IPBan Service**.

[:image:5_securing_your_data/securing_user_access/ip_ban_service/ip_ban_settings.png]
