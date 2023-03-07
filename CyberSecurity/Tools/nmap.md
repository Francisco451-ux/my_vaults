cae## scanning technique on the predefined list

```shell-session
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```

## Scan Multiple IPs

```shell-session
 sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5
```

**range in the respective octet**
```shell-session
sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5
```

```shell-session
sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 100 --reason 
```

```shell-session
sudo nmap 10.129.2.28 -p 445 --packet-trace -n --disable-arp-ping -Pn
```

Hard:
#### SYN-Scan of a Filtered Port
```shell-session
sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace
```

## Version
```shell-session
sudo nmap 10.10.113.49 -p- -sV -Pn -n --disable-arp-ping --packet-trace
```

```
sudo nmap 10.129.161.221 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace

Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2022-03-10 08:58 EST
SENT (0.0733s) TCP 10.10.15.155:41132 > 10.129.161.221:50000 S ttl=48 id=30790 iplen=44  seq=1866284190 win=1024 <mss 1460>
SENT (1.0748s) TCP 10.10.15.155:41133 > 10.129.161.221:50000 S ttl=58 id=7974 iplen=44  seq=1866218655 win=1024 <mss 1460>
Nmap scan report for 10.129.161.221
Host is up.

PORT      STATE    SERVICE
50000/tcp filtered ibm-db2
```

So, for example, we could use them to interact with the hosts of the internal network. As another example, we can use `TCP port 53` as a source port (`--source-port`) for our scans. If the administrator uses the firewall to control this port and does not filter IDS/IPS properly, our TCP packets will be trusted and passed through.
#### Connect To The Filtered Port
```
ncat -nv --source-port 53 10.129.161.221 50000
```

```
sudo nmap 10.129.2.48 -p445 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53

```

```
sudo nmap -sSU -p 53 --script dns-nsid 10.129.159.115

```