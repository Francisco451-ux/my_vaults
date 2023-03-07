The Sysinternals tools is a compilation of over 70+ Windows-based tools. Each of the tools falls into one of the following categories:

-   File and Disk Utilities
-   Networking Utilities
-   Process Utilities
-   Security Utilities
-   System Information
-   Miscellaneous

 Install the Sysinternals Suite
 
 **Sysinternals Utilities Index** page, [Download](https://docs.microsoft.com/en-us/sysinternals/downloads/)

For example, let's say you need tools to inspect Windows processes; then, you can navigate to the **Process Utilities** page, [process-utilities](https://docs.microsoft.com/en-us/sysinternals/downloads/process-utilities/), for all the tools that fall under this category.

`Há Pata`

After you download the zip file, you need to extract the files. After the files are extracted, the extra step, which is by choice, is to add the folder path to the environment variables. By doing so, you can launch the tools via the command line without navigating to the directory the tools reside in. 

**Environment Variables** can be edited from **System Properties**.

The System Properties can be launched via the command line by running `sysdm.cpl`. Click on the `Advanced` tab. 

![](https://assets.tryhackme.com/additional/sysinternals/env-variables.png)  

Select `Path` under `System Variables` and select Edit... then OK.

![](https://assets.tryhackme.com/additional/sysinternals/env-variables2.png)  

In the next screen select `New` and enter the folder path where the Sysinternals Suite was extracted to. Press OK to confirm the changes.

![](https://assets.tryhackme.com/additional/sysinternals/env-variables3.png)  

Open a new command prompt (elevated) to confirm that the Sysinternals Suite can be executed from any location.

Alternatively, a PowerShell module can download and install all of the Sysinternals tools. 

-   PowerShell command: `Download-SysInternalsTools C:\Tools\Sysint`


Using Sysinternals Live

File and Disk Utilities

**Sigcheck**

"**Sigcheck** is a command-line utility that shows file version number, timestamp information, and digital signature details, including certificate chains. It also includes an option to check a file’s status on VirusTotal, a site that performs automated file scanning against over 40 antivirus engines, and an option to upload a file for scanning." (**official definition**)


```
sigcheck -u -e C:\Windows\System32 -accepteula

```

**Streams**

"The NTFS file system provides applications the ability to create alternate data streams of information. By default, all data is stored in a file's main unnamed data stream, but by using the syntax 'file:stream', you are able to read and write to alternates." (**official definition**)

```
streams C:\Users\Administrator\Desktop\SysternalsSuite.zip -accepteula


```