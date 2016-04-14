### Messages Queues
The latest versions of Pydio come with an "instant messaging" service using WebSockets.

WebSockets enable the communication between a client and a server in real time, meaning that the client doesn't have to use heavy pull request to reload information it needs. In that context, it is the server that decides when and to which client it needs to send information.

By default, the Pydio backend implements "messaging" by storing events inside “queues” on the server (either in serialized files or in DB, depending on the Message Queuing configuration), and the clients are polling on a regular basis to check if some messages are intended to them. To avoid this permanent polling, we introduced a permanent full-duplex connexion between the browser and the server.

With the "instant messaging", Pydio is rid of the need to reload huge files list repeatedly, along with the authentication process required for each request, making it better, faster, stronger, without losing any bit of data safety.

### How it works
After logging in, each user seamlessly create a WebSocket that connects to the WebSocket Server through a specific port and a **public** channel. Each time the user enters a Workspace, a message is sent to the WebSocket public server to register and listen to the workspace channel he's browsing  __(through SocketIO for npm WebSocket servers, or the standard WebSocket JS API for php WebSocket servers)__. The user is identified upon connection and the WebSocket server checks the user's rights access to the workspace.

When a specific action occurs on the server (eg: a file is renamed by a user), Pydio triggers an action that sends a message to the WebSocket server through a **private** channel. The server sends along an admin secret key to identify, and only registered hosts can connect.

The WebSocket server then dispatch those messages to the different Workspaces channels that may be concerned.

The user will receive those messages and the browser will adapt the UI in near real time!

### Running the WebSocket server
In the latest version of Pydio (v6.4.2), you have the choice between using the PHP WebSocket Server, or use the NPM Socket.IO server. The use of NPM Socket.IO is recommended on production system with multiple users. 

Go to Settings, and under Application Parameters > Message Queues, setup the following information :

+ __WebSocket Server Type__ : choose between npm and phpws.
+ __WebSocket public connection host__ : the host where the WebSocket server can be reached from a public (eg Web) client context _(default: localhost)_
+ __WebSocket public connection port__ : the port used on the WebSocket server for a public (eg Web) client context _(default: 8090)_
+ __WebSocket public connection through SSL__ : secure the messages through SSL _(default: No)_
+ __WebSocket public handler path__ : the path used on the WebSocket server for a public (eg Web) client context _(default /public)_
+ __WebSocket private connection host__ : the host where the WebSocket server can be reached from a private (eg Pydio server) context (default: localhost)
+ __WebSocket private connection port__ : the port used on the WebSocket server for a private (eg Pydio server) context _(default: 8090)_
+ __WebSocket private handler path__ : the path used on the WebSocket server for a private (eg Pydio server) context _(default /private)_
+ __WebSocket private key__ : the key used to authentify messages from a Pydio server to a WebSocket server
+ __WebSocket private authorized host__ : the list of authorized servers that can send messages to the private channel _(default: localhost, 127.0.0.1)_

After changing the parameters, press Save (to right) for the changes to apply.

For the PHP WebSocket Server, you can start/stop the WebSocket server in the Administration Screens by pressing the buttons "Switch the WS Server ON"/"Switch the WS Server OFF". The current status of the server is displayed underneath the buttons.

For the NPM WebSocket Server, you need to manually start the server by entering the command displayed at the top of the section underneath the WebSocket Server Type.


BEFORE :

[:image-popup:6_pydio_advanced_configuration/websocket_before.png]

AFTER :

[:image-popup:6_pydio_advanced_configuration/websocket_after.png]
