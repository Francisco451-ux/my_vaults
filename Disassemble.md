
Inicair 
```
r2 -d -A pwn107.pwn107 

```

Printar o stack: 

```

pdf @ main
```

break point:

```
[0x7ff7daac0950]> db 0x5589c2200a36
[0x7ff7daac0950]> db 0x5589c2200a3b

```

Correr o programa em debug:

```
[0x7ff7daac0950]> dc

```


Print the stack:
```
 pxr @ rsp

```

Repite a executio:

 ```
ood

```

lançar um payload com o programa:

```
 r2 -R stdin=payload -d -A pwn108.pwn108

```

Print section got.plt:

```
pxr @ section..got.plt

```
