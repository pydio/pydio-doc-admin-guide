For security reason, it is highly recommended to run Pydio Cells with a dedicated "unprivileged" user. Never run Cells as "root"!

In this guide, we use a dedicated user **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m -s /bin/bash pydio
```
