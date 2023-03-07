-smal

```
gobuster dir -u hhttps://intent-drink-me.chals.io/ -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -x .php,.css,.html,.java,.js,.txt,.py -t 100 -o gobuster_s1.txt 

```

```
gobuster dir -u http://167.99.8.90:9009/ -w /usr/share/seclists/Fuzzing/4-digits-0000-9999.txt -x .php,.css,.html,.java,.js,.txt,.python -t 40 -o gobuster_s2.txt

```

```

gobuster dir -u http://193.57.159.27:38134/secrets/
 -w /usr/share/dirb/wordlists/common.txt -x .php
	
```

```

gobuster dir -u http://10.10.204.195:445/ 

-w /usr/share/dirb/wordlists/common.txt -x .php


```
