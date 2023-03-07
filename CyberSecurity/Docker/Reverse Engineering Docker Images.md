

[Install Dive](https://github.com/wagoodman/dive#installation)
[[Docker]]

#### 1º Passo Download files
```

docker pull docker-rodeo.thm:5000/dive/example

```

#### 2º Passo View images Docker
```
docker images
```

output:

```
REPOSITORY                             TAG       IMAGE ID       CREATED         SIZE
public.ecr.aws/h0w1j9u3/grinch-aoc     latest    f886f0052070   7 months ago    508MB
rustscan/rustscan                      latest    32635bbf7b6c   11 months ago   41.7MB
docker-rodeo.thm:5000/dive/challenge   latest    2a0a63ea5d88   19 months ago   111MB
docker-rodeo.thm:5000/dive/example     latest    398736241322   19 months ago   87.1MB


```

#### Using Dive to see Layers get commands on docker

```
dive 2a0a63ea5d88

```

output:
```
┃ ● Layers ┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ │ Current Layer Contents ├───────────────────────────────────────────
Cmp   Size  Command                                                   Permission     UID:GID       Size  Filetree
     63 MB  FROM bfaa92d6f14efe5                                      -rwxr-xr-x         0:0     238 kB  │   │   ├── find   
     745 B  set -xe   && echo '#!/bin/sh' > /usr/sbin/policy-rc.d  && -rwxr-xr-x         0:0      31 kB  │   │   ├── flock                 
       7 B  mkdir -p /run/systemd && echo 'docker' > /run/systemd/con -rwxr-xr-x         0:0      43 kB  │   │   ├── fmt   
     47 MB  apt-get -qq update &&   DEBIAN_FRONTEND=noninteractive    -rwxr-xr-x         0:0      35 kB  │   │   ├── fold                  
    398 kB  useradd -m uogctf -s /bin/bash &&   mkdir -p /home/uogctf -rwxr-xr-x         0:0      18 kB  │   │   ├── free  
     798 B  echo "uogctf ALL=(ALL,!root) NOPASSWD: /bin/bash" >> /etc -rwxr-xr-x         0:0      31 kB  │   │   ├── getconf
    4.0 kB  echo "root:uogctf" | chpasswd &&     echo "Successfully c -rwxr-xr-x         0:0      31 kB  │   │   ├── getent   
                                                                      -rwxr-xr-x         0:0      14 kB  │   │   ├── getopt                
│ Layer Details ├──────────────────────────────────────────────────── -rwxr-xr-x         0:0      76 kB  │   │   ├── gpasswd
                                                                      -rwxr-xr-x         0:0     437 kB  │   │   ├── gpgv                  
Tags:   (unavailable)                                                 -rwxr-xr-x         0:0      35 kB  │   │   ├── groups       
Id:     2b4ac5de5caa410142ad5338b0e99348cb02e2998f44eaaf4b5e852432266 -rwxr-xr-x         0:0      43 kB  │   │   ├── head 
26d                                                                   -rwxr-xr-x         0:0      31 kB  │   │   ├── hostid       
Digest: sha256:e245b7fac9f306a90cf5d474e037130f43795839c7474d9b6903ac -rwxrwxrwx         0:0        0 B  │   │   ├── i386 → setarch
0985409631                                                            -rwxr-xr-x         0:0      64 kB  │   │   ├── iconv   
Command:                                                              -rwxr-xr-x         0:0      43 kB  │   │   ├── id     
apt-get -qq update &&   DEBIAN_FRONTEND=noninteractive   apt-get -y - -rwxr-xr-x         0:0      60 kB  │   │   ├── infocmp 
-no-install-recommends -qq install   openssh-server   apt-utils   sud -rwxrwxrwx         0:0        0 B  │   │   ├── infotocap → tic
o=1.8.21p2-3ubuntu1 &&   mkdir -p /var/run/sshd &&   mkdir -p /root/. -rwxr-xr-x         0:0     146 kB  │   │   ├── install

```