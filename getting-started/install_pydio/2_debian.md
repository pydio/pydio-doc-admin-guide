## Install public key and repositories

Install apt-transport-https package:

> `sudo apt-get update && sudo apt-get upgrade`

> `sudo apt-get install apt-transport-https`

Install our public key (used to sign the packages) using the following command:

> `wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -`

Then edit /etc/apt/sources.list and add the following lines

> `deb https://download.pydio.com/pub/linux/debian/ jessie-backports main`

Now update your repositories list by running:

> `sudo apt-get update`

## Install Pydio Community Distribution

Now that the repositories are properly configured, you simply have to install pydio using:

> `apt-get install pydio`

After that step, you should be able to access pydio through your browser: http://YOUR_IP_ADDRESS/pydio/
