### Messages Queues
Since Pydio 5, an “instant messaging” system was introduced, to avoid permanent reloading of the files list, and a to send SERVER => CLIENTS events triggering various refreshes.

By default, the backend implements this feature by storing events inside “queues” on the server (either in serialized files or in DB, depending on the Message Queuing configuration), and the clients are polling on a regular basis to check if some messages are intended to them. To avoid this permanent polling, and when the client browser supports this, a WebSocket server was introduced to create a permanent, full-duplex connexion, between the browser and the server. This is based on the phpws project, a simple PHP implementation of a websocket server. We may provide more classical implementation in the future, based on Nodejs.

### Running the WebSocket server
In order to run the server, you will just have to make sure that the PHP CLI is available, and choose an open port on which the server can listen, and that the clients can access. Then, go to the Settings, under Global Configurations > Core Configs, “Message Queuing” Panel, and enter the necessary parameters :

+ WS Host/WS Port : IP & Port of the server, to which both the websocket server will try to bind its socket, and that Pydio clients will try to connect to. Current implementation may prevent proxying.
+ WS Path : the Pydio URI
+ WS Key : a secret key that will be passed to the websocket server at startup, and that will make sure that only this Pydio installation sends messages to this server.
Before saving, click the button “Switch the WS Server ON”, and wait until the “WS Status” label refreshes. Until it’s not turning to “ON”, your configuration is not correct. Once it’s ok, save this, and hard refresh the client (Ctrl+R or F5). By using Developer Tools network monitoring, you should now detect that there are no more many **_get_action=client_consume_channel_** requests polling, but a connexion opened with the Web Socket server.

BEFORE :

[:image-popup:websocket_server_activation/screenshot-2013-05-13-at-12-51-37.png]

AFTER :

[:image-popup:websocket_server_activation/screenshot-2013-05-13-at-12-59-07.png]
