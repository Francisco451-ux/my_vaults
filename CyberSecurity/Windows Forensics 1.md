## Windows Registry and Forensics

**Structure of the Registry:**

The registry on any Windows system contains the following five root keys:

1.  HKEY_CURRENT_USER
2.  HKEY_HKEY_USERS
3.  HKEY_LOCAL_MACHINE
4.  HKEY_CLASSES_ROOT
5.  HKEY_CURRENT_CONFIG

You can view these keys when you open the `regedit.exe` utility. To open the registry editor, press the Windows key.

## Accessing registry hives offline
If you are accessing a live system, you will be able to access the registry using regedit.exe, and you will be greeted with all of the standard root keys we learned about in the previous task. However, if you only have access to a disk image, you must know where the registry hives are located on the disk. The majority of these hives are located in the `C:\Windows\System32\Config` directory and are:

1.  **DEFAULT** (mounted on `HKEY_USERS\DEFAULT`)
2.  **SAM** (mounted on `HKEY_LOCAL_MACHINE\SAM`)
3.  **SECURITY** (mounted on `HKEY_LOCAL_MACHINE\Security`)
4.  **SOFTWARE** (mounted on `HKEY_LOCAL_MACHINE\Software`)
5.  **SYSTEM** (mounted on `HKEY_LOCAL_MACHINE\System`)


**Transaction Logs and Backups:**

Some other very vital sources of forensic data are the registry transaction logs and backups. The transaction logs can be considered as the journal of the changelog of the registry hive. Windows often uses transaction logs when writing data to registry hives. This means that the transaction logs can often have the latest changes in the registry that haven't made their way to the registry hives themselves. The transaction log for each hive is stored as a .LOG file in the same directory as the hive itself. It has the same name as the registry hive, but the extension is .LOG. For example, the transaction log for the SAM hive will be located in `C:\Windows\System32\Config` in the filename SAM.LOG. Sometimes there can be multiple transaction logs as well. In that case, they will have .LOG1, .LOG2 etc., as their extension. It is prudent to look at the transaction logs as well when performing registry forensics.

Registry backups are the opposite of Transaction logs. These are the backups of the registry hives located in the `C:\Windows\System32\Config` directory. These hives are copied to the `C:\Windows\System32\Config\RegBack` directory every ten days. It might be an excellent place to look if you suspect that some registry keys might have been deleted/modified recently.