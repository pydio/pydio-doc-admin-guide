Get started with Cells static binaries

1. **Get your database access ready** and login to your server as a dedicated user (see [Requirements](./requirements'))
   

2. **Download the binary** corresponding to your Cells edition and server architecture from the [Download Page](/en/download).
   
   On Linux/MacOSX, make sure to make the binary executable using `chmod +x cells`. 
   

3. **Go through Cells configuration** steps using either a browser or the command line (see below):
   
   `./cells configure`


4. **Start Cells** : web installer mode will restart automatically after configuration, otherwise use:
    
   `./cells start`
   

5. **Open your web browser** at https://localhost:8080 (or https://[server ip or domain]:8080).

-----
## Installation modes

### Web Installer

If you are on a desktop machine, the installer opens a web page with a wizard asking you to fill in various configuration parameters, including DB connection info and the login/password of the main administrator.

Click on the image below to see a screencast of the installation:

[:image:1_quick_start/installation/web-installer.gif]

After it completes, the server restarts automatically and you are good to go.

### Command line Installer

If you are more a shell person, you can perform the same steps using command-line (cli) prompts/answers. 
The image below shows a screencast of the CLI installation :

[:image:1_quick_start/installation/cli-installer.gif]

-----

## Troubleshooting

#### Connection URL 

The Cells gateway starts by default on `0.0.0.0:8080`.

By using 0.0.0.0, the gateway will allow incoming traffic to any IPv4/domain of the machine network interfaces.

If the port 8080 is busy (already used by another process), it will try other options.

A self-signed certificate is also automatically generated for supporting TLS connection out of the box (https).

For production use, or for a remotely accessed Cells, you can make the gateway listen to multiple ports on multiple IP addresses,
each with its own set of rules attached (TLS, Let's Encrypt, External URL)

 - At installation time using specific flags (`--bind`, `tls_xxx`): see the [man page of the configure command TODO](LINK TO DOC). 
 - With the dedicated tool `./cells configure sites`: see this [dedicated page for more information about the "sites" (TODO)](LINK TO PAGE).