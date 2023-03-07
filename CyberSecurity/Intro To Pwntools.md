cd
solucoes: 
https://github.com/dizmascyberlabs/IntroToPwntools

checksec:
```bash

[*] '/home/buzz/IntroToPwntools/IntroToPwntools/checksec/intro2pwn1'
    Arch:     i386-32-little
    RELRO:    Full RELRO
    Stack:    Canary found
    NX:       NX enabled
    PIE:      PIE enabled
buzz@intro2pwn:~/IntroToPwntools/IntroToPwntools/checksec$ checksec intro2pwn2
[*] '/home/buzz/IntroToPwntools/IntroToPwntools/checksec/intro2pwn2'
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX disabled
    PIE:      No PIE (0x8048000)
    RWX:      Has RWX segments

```


**RELRO** stands for Relocation Read-Only, which makes the global offset table (GOT) read-only after the linker resolves functions to it. The GOT is important for techniques such as the ret-to-libc attack, although this is outside the scope of this room. If you are interested, you can refer to this blog post: [https://www.redhat.com/en/blog/hardening-elf-binaries-using-relocation-read-only-relro](https://www.redhat.com/en/blog/hardening-elf-binaries-using-relocation-read-only-relro).

**Stack canaries** are tokens placed after a stack to detect a stack overflow. These were supposedly named after birds that coal miners brought down to mines to detect noxious fumes. Canaries were sensitive to the fumes, and so if they died, then the miners knew they needed to evacuate. On a less morbid note, stack canaries sit beside the stack in memory (where the program variables are stored), and if there is a stack overflow, then the canary will be corrupted. This allows the program to detect a buffer overflow and shut down. You can read more about stack canaries here: [https://www.sans.org/blog/stack-canaries-gingerly-sidestepping-the-cage/](https://www.sans.org/blog/stack-canaries-gingerly-sidestepping-the-cage/).

**NX** is short for non-executable. If this is enabled, then memory segments can be either writable or executable, but not both. This stops potential attackers from injecting their own malicious code (called shellcode) into the program, because something in a writable segment cannot be executed.  On the vulnerable binary, you may have noticed the extra line **RWX** that indicates that there are segments which can be read, written, and executed. See this Wikipedia article for more details: [https://en.wikipedia.org/wiki/Executable_space_protection](https://en.wikipedia.org/wiki/Executable_space_protection)

**PIE** stands for Position Independent Executable. This loads the program dependencies into random locations, so attacks that rely on memory layout are more difficult to conduct. Here is a good blog about this: [https://access.redhat.com/blogs/766093/posts/1975793](https://access.redhat.com/blogs/766093/posts/1975793)[](https://access.redhat.com/blogs/766093/posts/1975793)

If you want a good overview of each of the checksec tested qualities, I have found this guide to be useful: [https://blog.siphos.be/2011/07/high-level-explanation-on-some-binary-executable-security/](https://blog.siphos.be/2011/07/high-level-explanation-on-some-binary-executable-security/)



1C6F654E59A3B0C179D366AE&0

Kingston DataTraveler 2.0 USB Device