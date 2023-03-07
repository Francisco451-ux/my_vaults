#### Locating TShark

```shell-session
Avataris1210@htb[/htb]$ which tshark

Avataris1210@htb[/htb]$ tshark -D

Avataris1210@htb[/htb]$ tshark -i 1 -w /tmp/test.pcap

Capturi
```


#### Selecting an Interface & Writing to a File

```shell-session
sudo tshark -i eth0 -w /tmp/test.pcap
```


#### Applying Filters

```shell-session
sudo tshark -i eth0 -f "host 10.129.43.4"
```

#### The Statistics and Analyze Tabs
#### Statistics Tab
#### Analyze Tab

#### Following TCP Streams

To utilize this feature:

-   right-click on a packet from the stream we wish to recreate.
-   select follow → TCP
-   this will open a new window with the stream stitched back together. From here, we can see the entire conversation.


## Filter For A Specific TCP Stream




#### Extracting Data and Files From a Capture

To extract files from a stream:

-   stop your capture.
-   Select the File radial → Export → , then select the protocol format to extract from.
-   (DICOM, HTTP, SMB, etc.)





Exemplos de filtros ftp:

`ftp`
`ftp.request.command` 
`ftp-data`