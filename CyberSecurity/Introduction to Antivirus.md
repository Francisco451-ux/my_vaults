What is the sigtool tool output to generate an MD5 of the AV-Check.exe binary?
```

C:\Program Files\ClamAV>.\sigtool.exe --md5 C:\Users\thm\Desktop\Samples\AV-Check.exe
```

Use the strings tool to list all human-readable strings of the AV-Check binary. What is the flag?

```
C:\Users\thm\Desktop\Samples>strings AV-Check.exe | findstr "THM"
```