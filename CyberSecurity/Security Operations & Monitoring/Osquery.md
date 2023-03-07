**[Osquery](https://osquery.io/)** is an [open-source](https://github.com/osquery/osquery) tool created by **[Facebook](https://engineering.fb.com/2014/10/29/security/introducing-osquery/)**. With Osquery, Security Analysts, Incident Responders, Threat Hunters, etc., can query an endpoint (or multiple endpoints) using SQL syntax. Osquery can be installed on multiple platforms: Windows, Linux, macOS, and FreeBSD.

Some of the tools (open-source and commercial) that utilize Osquery are listed below.

-   **Alienvault**: The [AlienVault agent](https://otx.alienvault.com/endpoint-security/welcome) is based on Osquery. 
-   **Cisco**: Cisco AMP (Advanced Malware Protection) for endpoints utilize Osquery in [Cisco Orbital](https://orbital.amp.cisco.com/help/). 

Learning Osquery will be beneficial if you are looking to enter into this field or if you're already in the field and you're looking to level up your skills.


#### Interacting with the Osquery Shell

Console/shell, open CMD (or PowerShell) and run `osqueryi`
commands


```
osquery>.tables process
```


What is the Osquery version?
```
osquery>.version
```

What is the default output mode?

```
osquery>.show

```

What is the meta-command to set the output to show one value per line?
 ```
osquery>.help

```


#### Schema Documentation

Head over to the schema documentation [here](https://osquery.io/schema/4.7.0/).

#### Creating queries

`SELECT * FROM processes;`

```
osquery> `SELECT pid, name, path FROM processes;`
```

```
osquery>`SELECT count(*) from processes;`
```

```
osquery>`SELECT pid, name, path FROM processes WHERE name='lsass.exe';`
```



#### Linux and Osquery

What is the 'current_value' for kernel.osrelease?
```
osquery> SELECT * FROM kernel_info;
```

What is the uid for the bravo user?
```
osquery> SELECT username,uid from users where username= "bravo" ;
```

One of the users performed a 'Binary Padding' attack. What was the target file in the attack?
```
select * from shell_history;
```

There is a file that is categorized as malicious in one of the home directories. Query the Yara table to find this file. Use the sigfile which is saved in '/var/osquery/yara/scanner.yara'. Which file is it?

```
osquery> SELECT * FROM yara WHERE path="/path/filename" and sigfile="/var/osquery/yara/scanner.yara";
```



#### Windows and Osquery


What is the description for the Windows Defender Service?

```
select name,description from services where name like "WinD%";
```

There is another security agent on the Windows endpoint. What is the name of this agent?

```
select name,publisher from programs;

```

What is required with win_event_log_data?


How many sources are returned for win_event_log_channels?
```
osquery> select count (*) from win_event_log_channels;
```


What is the schema for win_event_log_data?

```
osquery> .schema win_event_log_data
```


The previous file scanned on the Linux endpoint with Yara is on the Windows endpoint.  What date/time was this file first detected? (Answer format: **YYYY-MM-DD HH:MM:SS)**

```
osquery> select eventid,datetime from win_event_log_data where source = "Microsoft-Windows-Windows Defender/Operational" and eventid like '1116'`
```


What is the query to find the first Sysmon event? Select only the event id, order by date/time, and limit the output to only 1 entry.

```

Query 1: find the sysmon Source `select * from win_event_log_channels where source like '%sysmon%';`
```

```
Query 2: `select eventid from win_event_log_data where source="Microsoft-Windows-Sysmon/Operational" order by datetime limit 1;`
```

SELECT eventid FROM win_event_log_data WHERE source="Microsoft-Windows-Sysmon/Operational" ORDER BY datetime asc LIMIT 1;

