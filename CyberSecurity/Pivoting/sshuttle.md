PraFirst of all we need to install sshuttle. On Kali this is as easy as using the `apt` package manager:  
`sudo apt install sshuttle`


The base command for connecting to a server with sshuttle is as follows:  
`sshuttle -r username@address subnet`


For example, in our fictional 172.16.0.x network with a compromised server at 172.16.0.5, the command may look something like this:  
`sshuttle -r user@172.16.0.5 172.16.0.0/24`

Rather than specifying subnets, we could also use the `-N` option which attempts to determine them automatically based on the compromised server's own routing table:  
`sshuttle -r username@address -N`


So, when using key-based authentication, the final command looks something like this:  
`sshuttle -r user@address --ssh-cmd "ssh -i KEYFILE" SUBNET`  

To use our example from before, the command would be:  
`sshuttle -r user@172.16.0.5 --ssh-cmd "ssh -i private_key" 172.16.0.0/24`


**Please Note:** When using sshuttle, you may encounter an error that looks like this:  
`client: Connected.   client_loop: send disconnect: Broken pipe   client: fatal: server died with error code 255`

To get around this, we tell sshuttle to exclude the compromised server from the subnet range using the `-x` switch.

To use our earlier example:  
`sshuttle -r user@172.16.0.5 172.16.0.0/24 -x 172.16.0.5`