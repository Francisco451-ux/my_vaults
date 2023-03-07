#### Linux Enumeration

```shell-session
ls /etc/*-release
```

```shell-session
cat /etc/os-release 
```

```shell-session
hostname
```

```shell-session
cat /etc/passwd
```

```shell-session
cat /etc/group
```


```shell-session
 sudo cat /etc/shadow
```

```shell-session
ls -lh /var/mail/
```

```shell-session
dpkg -l
```

```shell-session
who
```

```shell-session
whoami
```

```shell-session
w
```

```shell-session
id
```

```shell-session
last
```


### Networking
```shell-session
ip a s
```

```shell-session
cat /etc/resolv.conf
```

```shell-session
sudo netstat -plt
```

```shell-session
sudo netstat -atupn
```

```shell-session
sudo lsof -i
```

```shell-session
ps axf
```

```shell-session
 ps -ef | grep peter
```

#### Windows Enumeration



`dig -t AXFR DOMAIN_NAME @DNS_SERVER`

dig -t AXFR redteam.thm @10.10.74.104
