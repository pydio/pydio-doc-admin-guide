_In this guide you are going to see how you can launch Pydio Cells as a background service using [Supervisor](http://supervisord.org/), we will also provide you with all the steps and a simple configuration file thus allowing you to do that._

We are going to see the main Linux systems such as :

1. [Ubuntu and Debian]()
2. [CentOS]()  

## 1.Ubuntu and Debian
We will show you a simple Ubuntu & Debian configuration for Supervisor that will help you launch Pydio Cells as a background service.

### Requirements

### Supervisor
First you need supervisor installed to do that you can use the following command :
* `sudo apt-get install supervisor`

then after the installation make sure that supervisor is started :
* `sudo service supervisor status`

and if it's needed, start it with :
* `sudo service supervisor start`

Now you must add **cells** (the binary) to a supervisor conf file.
First create a file that must be located here `/etc/supervisor/conf.d/<the-file>.conf` named(for example) `cells.conf`, then you must add this (don't forget to change the `<path>`, and `user=` depending on your setup)

```
[program:cells]
command=/home/<path-to-binary> start
directory=/home/<folder-of-the-binary>       ; directory to cwd to before exec (def no cwd)
;umask=022                                   ; umask for process (default None)
;priority=999                                ; the relative start priority (default 999)
autostart=true                               ; start at supervisord start (default: true)
autorestart=unexpected                       ; whether/when to restart (default: unexpected)
startsecs=15                                 ; number of secs prog must stay running (def. 1)
startretries=5                               ; max # of serial start failures (default 3)
exitcodes=0,2                                ; 'expected' exit codes for process (default 0,2)
stopsignal=QUIT                              ; signal used to kill process (default TERM)
stopwaitsecs=10                              ; max num secs to wait b4 SIGKILL (default 10)
stopasgroup=false                            ; send stop signal to the UNIX process group (default false)
;killasgroup=false                           ; SIGKILL the UNIX process group (def false)
user=<user-launching-cells>                  ; setuid to this UNIX account to run the program

redirect_stderr=true                         ; redirect proc stderr to stdout (default false)
stdout_logfile=$HOME/.config/Pydio/Server/logs/cell.out
stdout_logfile_maxbytes=1MB                  ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=10                    ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB                  ; number of bytes in 'capturemode' (default 0)
;stdout_events_enabled=false                 ; emit events on stdout writes (default false)
stderr_logfile=$HOME/.config/Pydio/Server/logs/cell_err.log        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB                 ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10                   ; # of stderr logfile backups (default 10)
;stderr_capture_maxbytes=1MB                 ; number of bytes in 'capturemode' (default 0)
;stderr_events_enabled=false                 ; emit events on stderr writes (default false)
;environment=TMPDIR="/tmp",B="2"             ; process environment additions (def no adds)
;serverurl=AUTO                              ; override serverurl computation (childutils)

```

*Be advised that if you didn't put the right values for `<path>` and `user=` there might be an issue*

After that inform supervisor that it should now monitor this new program by using the following command :
* `sudo supervisorctl reread` (it will do it for every .conf file located within /etc/supervisor/conf.d )

then to enact the changes with :
* `sudo supervisorctl update`

Now you can monitor your program by using
* `sudo supervisorctl`

```
$ sudo supervisorctl
cells                             RUNNING   pid 3365, uptime 1:10:26
supervisor>
```

you can also also stop and start your programs after that for instance :
```
supervisor> stop cells
long_script: stopped
supervisor> start cells
long_script: started
supervisor> restart cells
long_script: stopped
long_script: started
```
or the status :
```
supervisor> status
cells                             RUNNING   pid 3365, uptime 1:13:07
supervisor>
```
use quit to leave the supervisor menu, and that should be it after all of those steps you should be able to have cells starting, auto-restarting by itself on background.

## 2.CentOS

### Monitoring Pydio Cells' services by Supervisord
The configuration is based on a system whose **pydio** account exists as in this [tutorial](https://github.com/pydio/cells/wiki/Install-CentOS)

- Install supervisor: `yum install supervisor`
- Create a new file /etc/supervisord.d/cell.ini with following content:

```
[program:cells]
command=/home/pydio/cells start
directory=/home/pydio       ; directory to cwd to before exec (def no cwd)
;umask=022                     ; umask for process (default None)
;priority=999                  ; the relative start priority (default 999)
autostart=true                ; start at supervisord start (default: true)
autorestart=unexpected        ; whether/when to restart (default: unexpected)
startsecs=15                   ; number of secs prog must stay running (def. 1)
startretries=5                ; max # of serial start failures (default 3)
exitcodes=0,2                 ; 'expected' exit codes for process (default 0,2)
stopsignal=QUIT               ; signal used to kill process (default TERM)
stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
stopasgroup=false             ; send stop signal to the UNIX process group (default false)
;killasgroup=false             ; SIGKILL the UNIX process group (def false)
user=pydio                 ; setuid to this UNIX account to run the program

redirect_stderr=true          ; redirect proc stderr to stdout (default false)
stdout_logfile=/home/pydio/.config/pydio/cells/logs/cell.out
stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=10     ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stdout_events_enabled=false   ; emit events on stdout writes (default false)
stderr_logfile=/home/pydio/.config/pydio/cells/logs/cell_err.log        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10     ; # of stderr logfile backups (default 10)
;stderr_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stderr_events_enabled=false   ; emit events on stderr writes (default false)
;environment=TMPDIR="/tmp",B="2"       ; process environment additions (def no adds)
;serverurl=AUTO                ; override serverurl computation (childutils)
```

- Enable supervisor start with system `systemctl enable supervisor && systemctl start supervisor`
- Update new program to supervisor: `supervisorctl update`
- Start cell program in supervisor: `supervisorctl start cell `

You can test this config by restarting the machine and **Pydio Cells** now is launched by supervisord. To watch the output, try this command:
`tail -f /home/cell/.config/Pydio/Server/logs/cell.out`

### Configure cells with systemd service

Create new file /etc/systemd/system/cells.service with content:

```
[Unit]
Description=Pydio Cells
Documentation=https://pydio.com
Wants=network-online.target
After=network-online.target
AssertFileIsExecutable=/home/pydio/cells
[Service]
WorkingDirectory=/home/pydio/.config/
User=pydio
Group=pydio
PermissionsStartOnly=true
#ExecStartPre=echo \"Start pydio cells-enterprise service\""
ExecStart=/home/pydio/cells start
Restart=on-failure
StandardOutput=journal
StandardError=inherit
LimitNOFILE=65536
TimeoutStopSec=5
KillSignal=INT
SendSIGKILL=yes
SuccessExitStatus=0
[Install]
WantedBy=multi-user.target

```

Then enable cells service:
`systemctl enable cells `
`systemctl start cells `

The output of cells service can be seen via journal service

`journalctl -f `