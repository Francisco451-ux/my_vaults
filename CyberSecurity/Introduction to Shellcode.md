Shellcode is a set of crafted machine code instructions that tell the vulnerable program to run additional functions and, in most cases, provide access to a system shell or create a reverse command shell.

Once the shellcode is injected into a process and executed by the vulnerable software or program, it modifies the code run flow to update registers and functions of the program to execute the attacker's code.

It is generally written in Assembly language and translated into hexadecimal opcodes (operational codes). Writing unique and custom shellcode helps in evading AV software significantly. But writing a custom shellcode requires excellent knowledge and skill in dealing with Assembly language, which is not an easy task! 

A Simple Shellcode!

In order to craft your own shellcode, a set of skills is required:

-   A decent understanding of x86 and x64 CPU architectures.
-   Assembly language.
-   Strong knowledge of programming languages such as C.
-   Familiarity with the Linux and Windows operating systems.

To generate our own shellcode, we need to write and extract bytes from the assembler machine code. For this task, we will be using the AttackBox to create a simple shellcode for Linux that writes the string "THM, Rocks!". The following assembly code uses two main functions:

-   System Write function (sys_write) to print out a string we choose.
-   System Exit function (sys_exit) to terminate the execution of the program.

To call those functions, we will use **syscalls**. A syscall is the way in which a program requests the kernel to do something. In this case, we will request the kernel to write a string to our screen, and the exit the program. Each operating system has a different calling convention regarding syscalls, meaning that to use the write in Linux, you'll probably use a different syscall than the one you'd use on Windows. For 64-bits Linux, you can call the needed functions from the kernel by setting up the following values:
  

The table above tells us what values we need to set in different processor registers to call the sys_write and sys_exit functions using syscalls. For 64-bits Linux, the rax register is used to indicate the function in the kernel we wish to call. Setting rax to 0x1 makes the kernel execute sys_write, and setting rax to 0x3c will make the kernel execute sys_exit. Each of the two functions require some parameters to work, which can be set through the rdi, rsi and rdx registers. You can find a complete reference of available 64-bits Linux syscalls [here](https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/).

For `sys_write`, the first parameter sent through `rdi` is the file descriptor to write to. The second parameter in `rsi` is a pointer to the string we want to print, and the third in `rdx` is the size of the string to print.

For `sys_exit`, rdi needs to be set to the exit code for the program. We will use the code 0, which means the program exited successfully.

Copy the following code to your AttackBox in a file called `thm.asm`: 

```markup
global _start

section .text
_start:
    jmp MESSAGE      ; 1) let's jump to MESSAGE

GOBACK:
    mov rax, 0x1
    mov rdi, 0x1
    pop rsi          ; 3) we are popping into `rsi`; now we have the
                     ; address of "THM, Rocks!\r\n"
    mov rdx, 0xd
    syscall

    mov rax, 0x3c
    mov rdi, 0x0
    syscall

MESSAGE:
    call GOBACK       ; 2) we are going back, since we used `call`, that means
                      ; the return address, which is, in this case, the address
                      ; of "THM, Rocks!\r\n", is pushed into the stack.
    db "THM, Rocks!", 0dh, 0ah
```

Let's explain the ASM code a bit more. First, our message string is stored at the end of the .text section. Since we need a pointer to that message to print it, we will jump to the call instruction before the message itself. When `call GOBACK` is executed, the address of the next instruction after call will be pushed into the stack, which corresponds to where our message is. Note that the 0dh, 0ah at the end of the message is the binary equivalent to a new line (\r\n).

Next, the program starts the GOBACK routine and prepares the required registers for our first sys_write() function.  

-   We specify the sys_write function by storing 1 in the rax register.
-   We set rdi to 1 to print out the string to the user's console (STDOUT).
-   We pop a pointer to our string, which was pushed when we called GOBACK and store it into rsi.
-   With the syscall instruction, we execute the sys_write function with the values we prepared.
-   For the next part, we do the same to call the sys_exit function, so we set 0x3c into the rax register and call the syscall function to exit the program.

Next, we compile and link the ASM code to create an x64 Linux executable file and finally execute the program.

Assembler and link our code

```
user@AttackBox$ nasm -f elf64 thm.asm 

user@AttackBox$ ld thm.o -o thm 

user@AttackBox$ ./thm THM,Rocks!
```
        

We used the `nasm` command to compile the asm file, specifying the `-f elf64` option to indicate we are compiling for 64-bits Linux. Notice that as a result we obtain a .o file, which contains object code, which needs to be linked in order to be a working executable file. The `ld` command is used to link the object and obtain the final executable. The `-o` option is used to specify the name of the output executable file.

Now that we have the compiled ASM program, let's extract the shellcode with the `objdump` command by dumping the .text section of the compiled binary.

Dump the .text section

```
user@AttackBox$ objdump -d thm 

thm:     file format elf64-x86-64   Disassembly of section .text:  0000000000400080 <_start>:   400080:	eb 1e                	jmp    4000a0   0000000000400082 :   400082:	b8 01 00 00 00       	mov    $0x1,%eax   400087:	bf 01 00 00 00       	mov    $0x1,%edi   40008c:	5e                   	pop    %rsi   40008d:	ba 0d 00 00 00       	mov    $0xd,%edx   400092:	0f 05                	syscall    400094:	b8 3c 00 00 00       	mov    $0x3c,%eax   400099:	bf 00 00 00 00       	mov    $0x0,%edi   40009e:	0f 05                	syscall   00000000004000a0 :   4000a0:	e8 dd ff ff ff       	callq  400082    4000a5:	54                   	push   %rsp   4000a6:	48                   	rex.W   4000a7:	4d 2c 20             	rex.WRB sub $0x20,%al   4000aa:	52                   	push   %rdx   4000ab:	6f                   	outsl  %ds:(%rsi),(%dx)   4000ac:	63 6b 73             	movslq 0x73(%rbx),%ebp   4000af:	21                   	.byte 0x21   4000b0:	0d                   	.byte 0xd   4000b1:	0a                   	.byte 0xa
```
        

Now we need to extract the hex value from the above output. To do that, we can use `objcopy` to dump the `.text` section into a new file called `thm.text` in a binary format as follows:

Extract the .text section

```
user@AttackBox$ objcopy -j .text -O binary thm thm.text
```        

The thm.text contains our shellcode in binary format, so to be able to use it, we will need to convert it to hex first. The `xxd` command has the `-i` option that will output the binary file in a C string directly:

Output the hex equivalent to our shellcode
```
user@AttackBox$ xxd -i thm.text 

unsigned char new_text[] = {   0xeb, 0x1e, 0xb8, 0x01, 0x00, 0x00, 0x00, 0xbf, 0x01, 0x00, 0x00, 0x00,   0x5e, 0xba, 0x0d, 0x00, 0x00, 0x00, 0x0f, 0x05, 0xb8, 0x3c, 0x00, 0x00,   0x00, 0xbf, 0x00, 0x00, 0x00, 0x00, 0x0f, 0x05, 0xe8, 0xdd, 0xff, 0xff,   0xff, 0x54, 0x48, 0x4d, 0x2c, 0x20, 0x52, 0x6f, 0x63, 0x6b, 0x73, 0x21,   0x0d, 0x0a }; unsigned int new_text_len = 50;
```
        

Finally, we have it, a formatted shellcode from our ASM assembly. That was fun! As we see, dedication and skills are required to generate shellcode for your work!

To confirm that the extracted shellcode works as we expected, we can execute our shellcode and inject it into a C program.

```c
#include <stdio.h>

int main(int argc, char **argv) {
    unsigned char message[] = {
        0xeb, 0x1e, 0xb8, 0x01, 0x00, 0x00, 0x00, 0xbf, 0x01, 0x00, 0x00, 0x00,
        0x5e, 0xba, 0x0d, 0x00, 0x00, 0x00, 0x0f, 0x05, 0xb8, 0x3c, 0x00, 0x00,
        0x00, 0xbf, 0x00, 0x00, 0x00, 0x00, 0x0f, 0x05, 0xe8, 0xdd, 0xff, 0xff,
        0xff, 0x54, 0x48, 0x4d, 0x2c, 0x20, 0x52, 0x6f, 0x63, 0x6b, 0x73, 0x21,
        0x0d, 0x0a
    };
    
    (*(void(*)())message)();
    return 0;
}
```

Then, we compile and execute it as follows,

Compiler our C program
```
user@AttackBox$ gcc -g -Wall -z execstack thm.c -o thmx u 
user@AttackBox$ ./thmx THM,Rocks!
   ```   

Nice! it works. Note that we compile the C program by disabling the NX protection, which may prevent us from executing the code correctly in the data segment or stack.

Understanding shellcodes and how they are created is essential for the following tasks, especially when dealing with encrypting and encoding the shellcode.