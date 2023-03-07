## Network Enumeration

```shell-session
netstat -na
```


```shell-session
arp -a
```

## Active Directory (AD) environment

```shell-session
systeminfo | findstr Domain
```

## Users and Groups Management

Use the Get-ADUser -Filter * -SearchBase command to list the available user accounts within THM OU in the thmredteam.com domain. How many users are available?

```
Get-ADUser -Filter * -SearchBase "OU=THM,DC=THMREDTEAM,DC=COM"
```

6

Once you run the previous command, what is the UserPrincipalName (email) of the admin account?



## Host Security Solution #1

### Antivirus Software (AV)

```Powershell
wmic /namespace:\\root\securitycenter2 path antivirusproduct
```


```Powershell
Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntivirusProduct
```

### Microsoft Windows Defender

```Powershell
Get-Service WinDefend
```


```Powershell
Get-MpComputerStatus | select RealTimeProtectionEnabled
```


```Powershell
Get-NetFirewallProfile | Format-Table Name, Enabled
```


```Powershell

Set-NetFirewallProfile -Profile Domain, Public, Private -Enabled False

Get-NetFirewallProfile | Format-Table Name, Enabled

```


```Powershell

 Get-NetFirewallRule | select DisplayName, Enabled, Description
```

```Powershell
Test-NetConnection -ComputerName 127.0.0.1 -Port 80



 (New-Object System.Net.Sockets.TcpClient("127.0.0.1", "80")).Connected

```

Enumerate the attached Windows machine and check whether the host-based firewall is enabled or not! (Y|N)
```Powershell
 Get-NetFirewallProfile | Format-Table Name, Enabled
 ```

N

Using PowerShell cmdlets such Get-MpThreat can provide us with threats details that have been detected using MS Defender. Run it and answer the following: What is the file name that causes this alert to record?

```Powershell
Get-MpThreat
```

PowerView.ps1


Enumerate the firewall rules of the attached Windows machine. What is the port that is allowed under the **THM-Connection** rule?

During the red team engagement, we have no clue what the **firewall blocks**. However, we can **take advantage of some PowerShell cmdlets** such as `Test-NetConnection` and `TcpClient`. Assume we know that a firewall is in place, and we need to test inbound connection without extra tools, then we can do the following:


```Powershell

 Get-NetFirewallRule | select DisplayName, Enabled, Description
```


## Host Security Solution #2


```Powershell
 Get-EventLog -List
```

### System Monitor (Sysmon)

**Look for a process or service**

```Powershell
Get-Process | Where-Object { $_.ProcessName -eq "Sysmon" }
```


**Look for services**
```Powershell

Get-CimInstance win32_service -Filter "Description = 'System Monitor service'"
```

**Checking the Windows registry**
```Powershell
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\Microsoft-Windows-Sysmon/Operational

```

**Try to find the sysmon configuration file**
```Powershell
findstr /si '<ProcessCreate onmatch="exclude">' C:\tools\*
```

### Host-based Intrusion Detection/Prevention System (HIDS/HIPS)

**HIDS** stands for Host-based Intrusion Detection System. It is software that has the ability to monitor and detect abnormal and malicious activities in a host. The primary purpose of HIDS is to detect suspicious activities and not to prevent them. There are two methods that the host-based or network intrusion detection system works, including:

-   Signature-based IDS - it looks at checksums and message authentication.
-   Anomaly-based IDS looks for unexpected activities, including abnormal bandwidth usage, protocols, and ports.



## Endpoint Detection and Response (EDR)

Even though an attacker successfully delivered their payload and bypassed EDR in receiving reverse shell, EDR is still running and monitors the system. It may block us from doing something else if it flags an alert.

We can use scripts for enumerating security products within the machine, such as [Invoke-EDRChecker](https://github.com/PwnDexter/Invoke-EDRChecker) and [SharpEDRChecker](https://github.com/PwnDexter/SharpEDRChecker). They check for commonly used Antivirus, EDR, logging monitor products by checking file metadata, processes, DLL loaded into current processes, Services, and drivers, directories.

## Applications and Services


### Installed Applications

```PowerShell
 wmic product get name,version
```

**PowerShell cmdlets, Get-ChildItem, as follow:**
```Powershell
Get-ChildItem -Hidden -Path C:\Users\kkidd\Desktop\
```


### Services and Process
Process discovery is an enumeration step to understand what the system provides. The red team should get information and details about running services and processes on a system. We need to understand as much as possible about our targets. This information could help us understand common software running on other systems in the network. For example, the compromised system may have a custom client application used for internal purposes. Custom internally developed software is the most common root cause of escalation vectors. Thus, it is worth digging more to get details about the current process.

### Sharing files and Printers

Sharing files and network resources is commonly used in personal and enterprise environments. System administrators misconfigure access permissions, and they may have useful information about other accounts and systems. For more information on printer hacking, we suggest trying out the following TryHackMe room: [Printer Hacking 101](https://tryhackme.com/room/printerhacking101).

### Internal services: DNS, local web applications, etc


```Powershell
net start
```

We can see a service with the name **THM Demo** which we want to know more about.  

Now let's look for the exact service name, which we need to find more information.
```Powershell
 wmic service where "name like 'THM Demo'" get Name,PathName
```

We find the file name and its path; now let's find more details using the Get-Process cmdlet.

```Powershell
Get-Process -Name thm-demo
```

Once we find its process ID, let's check if providing a network service by listing the listening ports within the system.
```Powershell

netstat -noa |findstr "LISTENING" |findstr "3212"

```

Answer:

Finally, we can see it is listening on port 8080. Now try to apply what we discussed and find the port number for THM Service. What is the port number?

```Powershell
net start

```

output:
THM Service 

```Powershell
wmic service where "name like 'THM Service'" get Name,PathName

```

output :
THM Service  c:\Windows\thm-service.exe
```Powershell
Get-Process -Name thm-Service

```
 output: 
 ID:2768

```Powershell
 netstat -noa |findstr "LISTENING" |findstr "2768"
```
output:

Port:13337

```Powershell
 wget 127.0.0.1:13337
```

Flag: THM{S3rv1cs_1s_3numerat37ed}

We mentioned that DNS service is a commonly used protocol in any active directory environment and network. The attached machine provides DNS services for AD. Let's enumerate the DNS by performing a zone transfer DNS and see if we can list all records.

Now enumerate the domain name of the domain controller, thmredteam.com, using the nslookup.exe, and perform a DNS zone transfer. **What is the flag for one of the records?**

 **enumerate the domain name of the domain controller**

```Powershell
Get-ADDomainController -filter *
```

```Powershell
Get-ADDomainController -Filter * | Select hostname, site
```


```Powershell
nslookup.exe
```

**DNS zone transfer**
```Powershell
server 10.10.79.65
```

```Powershell
ls -d thmredteam.com
```

THM{DNS-15-Enumerated!}