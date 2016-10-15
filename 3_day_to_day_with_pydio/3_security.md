[:image-popup:3_day_to_day_with_pydio/security.png]

The security Dashboard allows you to monitor who is currently connected to your Pydio server.

The first part shows all users currently connected to Pydio with an internet browser. If you detect any suspiscious activity here, you can kill this session in real-time, which means the user will be automatically disconnected, and won't be able to access the application for the next minute. You can then use the "People" dashboard to lock this user out of the application.

The second part shows the API-based connections over the last 24 hours. As this connection are not linked to a session, you cannot disconnect the user in real-time, but you can lock out this user via the People dashboard if necessary.