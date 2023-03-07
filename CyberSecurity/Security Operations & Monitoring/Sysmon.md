#### Installing Sysmon

PowerShell command: `Download-SysInternalsTools C:\Sysinternals`

To fully utilize Sysmon you will also need to download a Sysmon config or create your own config. We suggest downloading the [SwiftOnSecurity sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config). A Sysmon config will allow for further granular control over the logs as well as more detailed event tracing. In this room, we will be using both the SwiftOnSecurity configuration file as well as the [ION-Storm config file](https://github.com/ion-storm/sysmon-config/blob/develop/sysmonconfig-export.xml).

#### Starting Sysmon

Command Used: `Sysmon.exe -accepteula -i sysmonconfig-export.xml`




#### Filtering Events with PowerShell

```
Get-WinEvent -Path <Path to Log> -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="DestinationPort"] and */EventData/Data=4444'`

```




How many event ID 3 events are in `C:\Users\THM-Analyst\Desktop\Scenarios\Practice\Filtering.evtx`?

```
Get-WinEvent -Path C:\Users\THM-Analyst\Desktop\Scenarios\Practice\Filtering.evtx  -FilterXPath '*/System/EventID=3' | Measure-Object

```


What is the UTC time created of the first network event in `C:\Users\THM-Analyst\Desktop\Scenarios\Practice\Filtering.evtx`?


```
Get-WinEvent –Path ./Investigation-1.evtx -FilterXPath '*/System/EventID=3' -Oldest -MaxEvent 1 | Format-List

```


#### Hunting Metasploit
For more information about this technique and tools used check out [MITRE ATT&CK Software](https://attack.mitre.org/software/). 

For more information about how malware and payloads interact with the network check out the [Malware Common Ports Spreadsheet](https://docs.google.com/spreadsheets/d/17pSTDNpa0sf6pHeRhusvWG6rThciE8CsXTSlDUAZDyo). This will be covered in further depth in the Hunting Malware task.

Path: .\HuntingMetasploit.evtx
```
Get-WinEvent –Path <Path to evtx> -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="Image"]="C:\Users\THM-Threat\Downloads\shell.exe"' | Format-List
```

#### Detecting Mimikatz

Path: .\Hunting_Mimikatz.evtx
```
Get-WinEvent -Path <Path to Log> -FilterXPath '*/System/EventID=10 and */EventData/Data[@Name="TargetImage"] and */EventData/Data="C:\Windows\system32\lsass.exe"'
```

#### Hunting Malware


```
Get-WinEvent -Path <Path to Log> -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="DestinationPort"] and */EventData/Data=<Port>'`
```

#### Hunting Persistence
Hunting Alternate Data Streams
Open `C:\Users\THM-Analyst\Desktop\Scenarios\Practice\Hunting_ADS.evtx`

Detecting Remote Threads
Open `C:\Users\THM-Analyst\Desktop\Scenarios\Practice\Detecting_RemoteThreads.evtx`

  

Detecting Evasion Techniques with PowerShell

Syntax: `Get-WinEvent -Path <Path to Log> -FilterXPath '*/System/EventID=15'`

Syntax: `Get-WinEvent -Path <Path to Log> -FilterXPath '*/System/EventID=8'`



#### Practical Investigations

What is the full registry key of the USB device calling svchost.exe in Investigation 1?

```
 Get-WinEvent -Path .\Investigation-2.evtx -FilterXPath '*/System/EventID=13 and */EventData/Data[@Name="Image"]="C:\Windows\system32\svchost.exe"' | FL
 ```

What is the device name when being called by RawAccessRead in Investigation 1?

 ```
Get-WinEvent -Path .\Investigation-1.evtx -FilterXPath '*/System/EventID=9' | fl
 ```

What is the first exe the process executes in Investigation 1?

 ```
Get-WinEvent -Path .\Investigation-1.evtx -FilterXPath '*/System/EventID=1 and */EventData/Data[@Name="Image"]' | fl
 ```

Resto -> EventViewer
