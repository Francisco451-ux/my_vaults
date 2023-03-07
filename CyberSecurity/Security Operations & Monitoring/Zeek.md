ear

Zeek Outputs  

As mentioned before, Zeek provides 50+ log files under seven different categories, which are helpful in various areas such as traffic monitoring, intrusion detection, threat hunting and web analytics. This section is not intended to discuss the logs in-depth. The logs are covered in **TASK 3**.  

  Once you run Zeek, it will automatically start investigating the traffic or the given pcap file and generate logs automatically. Once you process a pcap with Zeek, it will create the logs in the working directory. If you run the Zeek as a service, your logs will be located in the default log path. The default log path is: `/opt/zeek/logs/`

**Working with Zeek**

There are two operation options for Zeek. The first one is running it as a service, and the second option is running the Zeek against a pcap. Before starting working with Zeek, let's check the version of the Zeek instance with the following command: `zeek -v`

Now we are sure that we have Zeek installed. Let's start the Zeek as a service! To do this, we need to use the "ZeekControl" module, as shown below. The "ZeekControl" module requires superuser permissions to use. You can elevate the session privileges and switch to the superuser account to examine the generated log files with the following command: `sudo su`

  
Here we can manage the Zeek service and view the status of the service. Primary management of the Zeek service is done with three commands; "status", "start", and "stop". 

  

ZeekControl Module
```


           `root@ubuntu$ zeekctl Welcome to ZeekControl 2.X.0 [ZeekControl] > status Name         Type       Host          Status    Pid    Started zeek         standalone localhost     stopped [ZeekControl] > start starting zeek ... [ZeekControl] > status Name         Type       Host          Status    Pid    Started zeek         standalone localhost     running   2541   13 Mar 18:25:08 [ZeekControl] > stop stopping zeek ... [ZeekControl] > status Name         Type       Host          Status    Pid    Started zeek         standalone localhost     stopped`
        

You can also use the "ZeekControl" mode with the following commands as well;  

-   `zeekctl status`
-   `zeekctl start` 
-   `zeekctl stop`

```

Para ver os ficheiro logs +-
```

root@ubuntu$ zeek -C -r sample.pcap

```


**Parameter**

**Description**

**-r**

 Reading option, read/process a pcap file.

**-C**

 Ignoring checksum errors.

**-v**

 Version information.

**zeekctl**

ZeekControl module.


#### ZEEK-CUT

In addition to Linux command-line tools, one auxiliary program called `zeek-cut` reduces the effort of extracting specific columns from log files. Each log file provides "field names" in the beginning. This information will help you while using `zeek-cut`. Make sure that you use the "fields" and not the "types".

https://darkdefender.medium.com/the-zeek-cut-cheat-sheet-d16663439ef4

![[Pasted image 20220801124046.png]]

Para ver melhor o ficheiro utilizamos a ferramenta zeek-cut

```markup
root@ubuntu$ cat conn.log | zeek-cut uid proto id.orig_h id.orig_p id.resp_h id.resp_p 
```


#### Zeek Signatures

Zeek supports signatures to have rules and event correlations to find noteworthy activities on the network.


