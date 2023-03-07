[[Stack-Based Buffer Overflows on Linux x86]]

GDB - EIP

```shell-session
(gdb) info registers eip

eip            0x69423569	0x69423569
```

GDB - EBP

```shell-session
(gdb) info registers ebp

ebp           0x55555555	0x55555555
```

#### Data registers

**32-bit Register**

**64-bit Register**

**Description**

`EAX`

`RAX`

Accumulator is used in input/output and for arithmetic operations

`EBX`

`RBX`

Base is used in indexed addressing

`ECX`

`RCX`

Counter is used to rotate instructions and count loops

`EDX`

`RDX`

Data is used for I/O and in arithmetic operations for multiply and divide operations involving large values

---

#### Pointer registers

**32-bit Register**

**64-bit Register**

**Description**

`EIP`

`RIP`

Instruction Pointer stores the offset address of the next instruction to be executed

`ESP`

`RSP`

Stack Pointer points to the top of the stack

`EBP`

`RBP`

Base Pointer is also known as `Stack Base Pointer` or `Frame Pointer` thats points to the base of the stack

#### Index registers

**Register 32-bit**

**Register 64-bit**

**Description**

`ESI`

`RSI`

Source Index is used as a pointer from a source for string operations

`EDI`

`RDI`

Destination is used as a pointer to a destination for string operations
#### Compile in 64-bit Format
```shell-session
gcc bow.c -o bow64 -fno-stack-protector -z execstack -m64
```

```shell-session
file bow64 | tr "," "\n"
```

Prologue

```shell-session
(gdb) disas bowfunc 

Dump of assembler code for function bowfunc:
   0x0000054d <+0>:	    push   ebp       # <---- 1. Stores previous EBP
   0x0000054e <+1>:	    mov    ebp,esp   # <---- 2. Creates new Stack Frame
   0x00000550 <+3>:	    push   ebx
   0x00000551 <+4>:	    sub    esp,0x404 # <---- 3. Moves ESP to the top
   <...SNIP...>
   0x00000580 <+51>:	leave  
   0x00000581 <+52>:	ret    
```

Epilogue

```shell-session
(gdb) disas bowfunc 

Dump of assembler code for function bowfunc:
   0x0000054d <+0>:	    push   ebp       
   0x0000054e <+1>:	    mov    ebp,esp   
   0x00000550 <+3>:	    push   ebx
   0x00000551 <+4>:	    sub    esp,0x404 
   <...SNIP...>
   0x00000580 <+51>:	leave  # <----------------------
   0x00000581 <+52>:	ret    # <--- Leave stack frame
```

## Endianness
Now, let us look at an example with the following values:

-   Address: `0xffff0000`
-   Word: `\xAA\xBB\xCC\xDD`

**Memory Address**

**0xffff0000**

**0xffff0001**

**0xffff0002**

**0xffff0003**

Big-Endian

AA

BB

CC

DD

Little-Endian

DD

CC

BB

AA

This is very important for us to enter our code in the right order later when we have to tell the CPU to which address it should point.