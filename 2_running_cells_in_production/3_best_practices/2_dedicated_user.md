
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v3. Looking for <a href="https://pydio.com/en/docs/cells/v4/quick-start">Pydio Cells v4 docs?</a></span>
</div>


For security reason, it is highly recommended to run Pydio Cells with a dedicated "unprivileged" user. Never run Cells as "root"!

In this guide, we use a dedicated user **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m -s /bin/bash pydio
```
