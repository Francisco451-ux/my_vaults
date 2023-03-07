#### Install Tcpdump

```shell-session
sudo apt install tcpdump 
```


#### Listing Available Interfaces
```shell-session
sudo tcpdump -D
```

#### Choosing an Interface to Capture From

```shell-session
sudo tcpdump -i eth0
```

#### Disable Name Resolution

```shell-session
sudo tcpdump -i eth0 -nn
```

#### Display the Ethernet Header

```shell-session
sudo tcpdump -i eth0 -e
```

#### Include ASCII and Hex Output

```shell-session
sudo tcpdump -i eth0 -X
```


#### Tcpdump Switch Combinations

```shell-session
sudo tcpdump -i eth0 -nnvXX
```

## Input/Output with Tcpdump

```shell-session
sudo tcpdump -i eth0 -w ~/output.pcap
```


#### Reading Output From a File

```shell-session
 sudo tcpdump -r ~/output.pcap
```


`Utilize Basic Capture Filters.`

```shell-session
tcpdump -i [interface name or #] -vX
```

`Save a Capture to a .PCAP file.`

```shell-session
tcpdump -i [interface name or #] -nvw [/path/of/filename.pcap]
```


`Read the Capture from a .PCAP file.`

```shell-session
tcpdump -nnSXr [file/to/read.pcap]
```


What TCPDump switch will allow us to pipe the contents of a pcap file out to another function such as 'grep'?