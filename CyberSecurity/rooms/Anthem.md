Enumeration: 

```
nmap -sC -sV -p- -Pn 10.10.182.112 
```

```
gobuster dir -u http://134.209.16.75:30377 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x php,txt,html -t 100 -o gobuster_s1.txt 
```


```
/search               (Status: 200) [Size: 3422]
/blog                 (Status: 200) [Size: 5399]
/categories           (Status: 200) [Size: 3546]
/authors              (Status: 200) [Size: 4120]
/Search               (Status: 200) [Size: 3472]
/tags                 (Status: 200) [Size: 3599]
/install              (Status: 302) [Size: 126] [--> /umbraco/]
/Blog                 (Status: 200) [Size: 5399]
/RSS                  (Status: 200) [Size: 1877]
/Archive              (Status: 301) [Size: 123] [--> /blog/]
/SiteMap              (Status: 200) [Size: 1065]
/robots.txt           (Status: 200) [Size: 192]
/siteMap              (Status: 200) [Size: 1065]
/INSTALL              (Status: 302) [Size: 126] [--> /umbraco/]
/Sitemap              (Status: 200) [Size: 1065]
/1073                 (Status: 200) [Size: 5399]
/Rss                  (Status: 200) [Size: 1877]
/Categories           (Status: 200) [Size: 3546]
/1074                 (Status: 301) [Size: 118] [--> /]
/1078                 (Status: 200) [Size: 6163]
/Authors              (Status: 200) [Size: 4075]
/1075                 (Status: 200) [Size: 4075]
/1079                 (Status: 200) [Size: 6223]
/1076                 (Status: 200) [Size: 5026]

```

[http://10.10.29.47/umbraco/](http://10.10.29.47/umbraco/)
 
Administrator: Solomon Grundy

email: SG@anthem.com

password: UmbracoIsTheBest!