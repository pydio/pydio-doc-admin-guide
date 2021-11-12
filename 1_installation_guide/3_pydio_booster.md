<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP). Time to move to <a href="https://pydio.com/en/docs/administration-guides">Pydio Cells</a>!</span>
</div>

Pydio Booster is a unique tool developed to easily boost your installation performance, by overriding inherent limitation of the PHP language:  it is written in Go and runs as a standalone server application that will communicate with the core pydio layer.

Main benefits are :

 - WebSocket out of the box: constant polling of the servers by the various Pydio "clients" (web browsers, PydioSync applications) putting a load on the system just to get the "last news" (any new file added? any new action by current user?). To avoid that, Pydio Booster provides a rock-solid, cross-platform, easy-to-deploy websocket server: clients can keep an open connection to this service and get events in real-time, with zero-impact on the main server CPU.
 - Overcome PHP upload limitations: when it comes to uploads, PHP is often a bottleneck in term of maximum file size or simply when it comes to writing files to underlying storage. Pydio Booster can take care of that, and just inform the php server when a new file has been added to storage! This is currently only working with a local (or mounted) file system, or with S3 object storages. More to come.
 - No more need for this complex CRONtab entry (or windows scheduling task) to fully use Pydio Scheduler. Booster will handle the "master" trigger for you.
 - Booster can also take care of the download of the files, removing the burden on the php server.

[:summary]