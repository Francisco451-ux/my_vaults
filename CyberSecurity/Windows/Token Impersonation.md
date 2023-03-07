#/windows/exploit/Hot_Potato ^920cdc

Hot Potato: Steps 1 to 4

Step 1: The target system uses the Web Proxy Auto-Discovery (WPAD) protocol to locate its update server.  
Step 2: This request is intercepted by the exploit, which sends a response redirecting the target system to a port on 127.0.0.1.  
Step 3: The target system will ask for a proxy configuration file (wpad.dat).  
Step 4: A malicious wpad.dat file is sent to the target.  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/603df7900d7b6f1dff18b0bd/room-content/81f88d6e43b595617be991de29484c7c.png)  

  

Hot Potato: Steps 5 to 8

Step 5: The target system tries to connect to the proxy (now set by the malicious wpad.dat file sent on the previous step).  
Step 6: The exploit will ask the target system to perform an NTLM authentication.  
Step 7: The target system sends an NTLM handshake.  
Step 8: The handshake received is relayed to the SMB service with a request to create a process. This process will have the privilege level of the service targeted, which would typically be "NT AUTHORITY\SYSTEM".  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/603df7900d7b6f1dff18b0bd/room-content/ac443f75a1851fb178ed4ea7c85f90e6.png)  

All requests are sent and received within the target system.

Microsoft published an update in the MS16-075 security bulletin to mitigate this exploit, and Hot Potato was followed by Rotten Potato. Rotten Potato uses a similar approach but leverages RPC.

Which "Potato" version you can use will vary depending on the target system's version, patch level and network connection limitation. While "Hot Potato" works within the target system, other versions may require network access over specific ports.