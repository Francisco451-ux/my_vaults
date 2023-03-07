Option 1: Install from Kali/OpenVAS repositories

Installing from repositories can sometimes be very simple or it can be a very painful process. For OpenVAS, the installation ranges in difficulty and can require many configurations ran. For more information about this option check out the guides below.

[https://websiteforstudents.com/how-to-install-and-configure-openvas-on-ubuntu-18-04-16-04/](https://websiteforstudents.com/how-to-install-and-configure-openvas-on-ubuntu-18-04-16-04/)[](https://websiteforstudents.com/how-to-install-and-configure-openvas-on-ubuntu-18-04-16-04/)

[https://www.agix.com.au/installing-openvas-on-kali-in-2020/](https://www.agix.com.au/installing-openvas-on-kali-in-2020/)

Option 2: Install from Source

Installing from source is the least preferred option for beginners and the least optimized way of installing OpenVAS due to prerequisites and make errors. For more information about installing from source look at the [INSTALL.MD](https://github.com/greenbone/openvas-scanner/blob/master/INSTALL.md).

Option 3: Run from Docker (Preferred)

Docker is by far the easiest of all three installation methods and only requires one command to be run to get the client started. For this installation procedure, you will need docker installed. 

For more information about this information procedure checkout the openvas-docker project on [GitHub](https://github.com/mikesplain/openvas-docker) and [DockerHub](https://hub.docker.com/r/mikesplain/openvas/dockerfile). 

1. `apt install docker.io`

2. `docker run -d -p 443:443 --name openvas mikesplain/openvas`