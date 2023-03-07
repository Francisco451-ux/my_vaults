[[Stack-Based Buffer Overflows on Linux x86]]
#### Create Pattern
```shell-session

/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 1200 > pattern.txt

```

run on gdb:

```shell-session
 run $(python -c "print 'Aa0Aa1Aa2Aa3Aa4Aa5...<SNIP>...Bn6Bn7Bn8Bn9'") 
```

output:
```shell-session
Program received signal SIGSEGV, Segmentation fault.
0x69423569 in ??
```

sayfriendandcomein
SAYFRIENDANDCOMEIN
#### GDB - Offset
```shell-session
Avataris1210@htb[/htb]$ /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 0x69423569

[*] Exact match at offset 1036
```

```shell-session
run $(python -c "print '\x55' * 18 + '\x66' * 4")
```
output:

```shell-session
Program received signal SIGSEGV, Segmentation fault.
0x66666666 in ?? ()
```

Nota: Next, we have to **find out how much space we have for our shellcode**, which then executes the commands we intend