#/windows/taskmanager

The columns are very minimal. The columns **Name**, **Status**, **CPU**, and **Memory,** are the only ones visible. To view more columns, right-click on any of the column headers to open more options.

`**Process Hacker**`  [download](https://processhacker.sourceforge.io/)

`**Process Explorer**`  [download](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer#installation)

Aside from Task Manager, it would be best if you also familiarize yourself with the command-line equivalent of obtaining information about the running processes on a Windows system: `tasklist`, `Get-Process` or `ps` (PowerShell), and `wmic`.

#windows/System/kernel

The official definition from Windows Internals 6th Edition:

"_The System process (process ID 4) is the home for a special kind of thread that runs only in kernel mode a kernel-mode system thread. System threads have all the attributes and contexts of regular user-mode threads (such as a hardware context, priority, and so on) but are different in that they run only in kernel-mode executing code loaded in system space, whether that is in Ntoskrnl.exe or in any other loaded device driver. In addition, system threads don't have a user process address space and hence must allocate any dynamic storage from operating system memory heaps, such as a paged or nonpaged pool._"

Technically this is correct.  Notice that Process Hacker **confirms this is legit '(Verified) Microsoft Windows. **

  
What is **unusual behavior** for this process?

-   A parent process (aside from System Idle Process (0))
-   Multiple instances of System. (Should only be 1 instance) 
-   A different PID. (Remember that the PID will always be PID 4)
-   Not running in Session 0

#/windows/processes/smss_exe

The next process is **smss.exe** (**Session Manager Subsystem**). This process, also known as the **Windows Session Manager**, is responsible for creating new sessions. This is the first user-mode process started by the kernel.

  This process starts the kernel mode and user mode of the Windows subsystem (you can read more about the NT Architecture [here](https://en.wikipedia.org/wiki/Architecture_of_Windows_NT)). This subsystem includes win32k.sys (kernel mode), winsrv.dll (user mode), and csrss.exe (user mode). 

  Smss.exe starts csrss.exe (Windows subsystem) and wininit.exe in Session `0`, an isolated Windows session for the operating system, and **csrss.exe** and **winlogon.exe** for Session `1`, which is the user session. The first child instance creates child instances in new sessions. This is done by smss.exe copying itself into the new session and self-terminating. You can read more about this process [here](https://en.wikipedia.org/wiki/Session_Manager_Subsystem).

**SMSS is also responsible for creating environment variables, virtual memory paging files and starts winlogon.exe (the Windows Logon Manager).**

Process Hacker -> smss.exe -> properties -> general

What is unusual?
-   A different parent process other than System(4)
-   Image path is different from C:\Windows\System32
-   More than 1 running process. (children self-terminate and exit after each new session)
-   User is not SYSTEM
-   Unexpected registry entries for Subsystem

#/windows/processes/csrss_exe

Notice what is shown for the parent process for these 2 processes. Remember these processes are spawned by `smss.exe` which self-terminates itself.  

  **Image Path**:  %SystemRoot%\System32\csrss.exe

**Parent Process**:  Created by an instance of smss.exe

**Number of Instances**:  Two or more

**User Account**:  Local System

**Start Time**:  Within seconds of boot time for the first 2 instances (for Session 0 and 1).  Start times for additional instances occur as new sessions are created, although often only Sessions 0 and 1 are created.

  What is unusual?

-   An actual parent process. (smss.exe calls this process and self-terminates)
-   Image file path other than   `C:\Windows\System32`
-   Subtle misspellings to hide rogue process masquerading as csrss.exe in plain sight
-   User is not SYSTEM

#/windows/processes/wininit_exe

The **Windows Initialization Process**, **wininit.exe**, is responsible for launching services.exe (Service Control Manager), lsass.exe (Local Security Authority), and lsaiso.exe within Session 0. This is another critical Windows process that runs in the background, along with its child processes.

-  Note: lsaiso.exe is a process associated with **Credential Guard and Key Guard**. You will only see this process if Credential Guard is enabled.

**Image Path**:  %SystemRoot%\System32\wininit.exe
**Parent Process**:  Created by an instance of smss.exe
**Number of Instances**:  One
**User Account**:  Local System
**Start Time**:  Within seconds of boot time

  What is unusual? #unusual ^f29606

-   An actual parent process. (smss.exe calls this process and self-terminates)
-   Image file path other than C:\Windows\System32
-   Subtle misspellings to hide rogue process in plain sight
-   Multiple running instances
-   Not running as SYSTEM


#/windows/processes/wininit_exe/services_exe

Resgistry Editor:

- Information regarding services is stored in the registry, `HKLM\System\CurrentControlSet\Services`


This process also loads device drivers marked as auto-start into memory.          
- When a user logs into a machine successfully, this process is responsible for setting the value of the Last Known Good control set (Last Known Good Configuration), `HKLM\System\Select\LastKnownGood`, to that of the CurrentControlSet.

  This process is the parent to several other key processes: svchost.exe, `spoolsv.exe`, `msmpeng.exe`, `dllhost.exe`, to name a few. You can read more about this process [here](https://en.wikipedia.org/wiki/Service_Control_Manager).
  
    What is unusual?
	[[Core Windows Processes#^f29606]]
	
	
	#/windows/processes/wininit_exe/services_exe/svchost_exe
	
	The **Service Host** (Host Process for Windows Services), or **svchost.exe**, is responsible for hosting and managing Windows services.
	
	The services running in this process are implemented as DLLs. The DLL to implement is stored in the registry for the service under the `Parameters` subkey in `ServiceDLL`. The full path is `HKLM\SYSTEM\CurrentControlSet\Services\SERVICE NAME\Parameters`.
	
	Also, notice how it is structured. There is a key identifier in the binary path. That identifier is `-k` . This is how a legitimate svchost.exe process is called
	
	
	Since svchost.exe will always have multiple running processes on any Windows system, this process has been a target for malicious use. Adversaries **create malware to masquerade** as this process and try to hide **amongst the legitimate** svchost.exe processes. They can name the malware `svchost.exe` or misspell it slightly, such as scvhost.exe. By doing so the intention is to go under the radar. Another tactic is to install/call a malicious service (DLL).  

  Extra reading - Hexacorn Blog ([https://www.hexacorn.com/blog/2015/12/18/the-typographical-and-homomorphic-abuse-of-svchost-exe-and-other-popular-file-names/](https://www.hexacorn.com/blog/2015/12/18/the-typographical-and-homomorphic-abuse-of-svchost-exe-and-other-popular-file-names/))
  
  **age Path**: %SystemRoot%\System32\svchost.exe
**Parent Process**: services.exe
**Number of Instances**: Many
**User Account**: Varies (SYSTEM, Network Service, Local Service) depending on the svchost.exe instance. In Windows 10 some instances can run as the logged-in user.
**Start Time**: Typically within seconds of boot time. Other instances can be started after boot

  What is unusual?

-   A parent process other than services.exe
-   Image file path other than C:\Windows\System32
-   Subtle misspellings to hide rogue process in plain sight
-   The absence of the -k parameter

#/windows/processes/lsass_exe

"Local Security Authority Subsystem Service (**LSASS**) is a process in Microsoft Windows operating systems that is responsible for enforcing the security policy on the system. It verifies users logging on to a Windows computer or server, handles password changes, and creates access tokens. It also writes to the Windows Security Log."

It creates security tokens for SAM (Security Account Manager), AD (Active Directory), and NETLOGON. It uses authentication packages specified in `HKLM\System\CurrentControlSet\Control\Lsa`.

his is another process adversaries target. Common tools such as **mimikatz** is used to dump credentials or they mimic this process to hide in plain sight. Again, they do this by either naming their malware by this process name or simply misspelling the malware slightly.

**Image Path**:  %SystemRoot%\System32\lsass.exe
**Parent Process**:  wininit.exe
**Number of Instances**:  One
**User Account**:  Local System
**Start Time**:  Within seconds of boot time

  What is unusual?

-   A parent process other than wininit.exe
-   Image file path other than C:\Windows\System32
-   Subtle misspellings to hide rogue process in plain sight
-   Multiple running instances
-   Not running as SYSTEM

#/windows/processes/winlogon_exe

The **Windows Logon**, **winlogon.exe**, is responsible for handling the **Secure Attention Sequence** (SAS). This is the ALT+CTRL+DELETE key combination users press to enter their username & password.

This process is also responsible for loading the user profile. This is done by loading the user's NTUSER.DAT into HKCU and via userinit.exe loads the user's shell. Read more about this process [here](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc939862(v=technet.10)?redirectedfrom=MSDN).

Remember from earlier sections, `smss.exe` launches this process along with a copy of csrss.exe within Session 1.

#/windows/processes/explorer_exe

the **Windows Explorer**, **explorer.exe**. This is the process that gives the user access to their folders and files. It also provides functionality to other features such as the Start Menu, Taskbar, etc.

As mentioned previously, the Winlogon process runs userinit.exe, which launches the value in `HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell`. Userinit.exe exits after spawning explorer.exe. Because of this, the parent process is non-existent.

**Image Path**:  %SystemRoot%\explorer.exe
**Parent Process**:  Created by userinit.exe and exits
**Number of Instances**:  One or more per interactively logged-in user
**User Account**:  Logged-in user(s)
**Start Tim**e:  First instance when the first interactive user logon session begins

  What is unusual?

-   An actual parent process. (userinit.exe calls this process and exits)
-   Image file path other than C:\Windows
-   Running as an unknown user
-   Subtle misspellings to hide rogue process in plain sight
-   Outbound TCP/IP connections