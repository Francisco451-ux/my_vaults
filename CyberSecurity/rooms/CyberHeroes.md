```shell-session
ffuf -w /usr/share/seclists/Usernames/Names/names.txt -X POST -d "uname=FUZZ&pass=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.75.43/login.html -mr "Incorrect Password, try again.. you got this hacker !"
```

Burpsuit 

autualizar a pagina de login e aparece esta funcao no intercpt request
```

 <script>
    function authenticate() {
      a = document.getElementById('uname')
      b = document.getElementById('pass')
      const RevereString = str => [...str].reverse().join('');
      if (a.value=="h3ck3rBoi" & b.value==RevereString("54321@terceSrepuS")) { 
        var xhttp = new XMLHttpRequest();
        xhttp.onreadystatechange = function() {
          if (this.readyState == 4 && this.status == 200) {
            document.getElementById("flag").innerHTML = this.responseText ;
            document.getElementById("todel").innerHTML = "";
            document.getElementById("rm").remove() ;
          }
        };
        xhttp.open("GET", "RandomLo0o0o0o0o0o0o0o0o0o0gpath12345_Flag_"+a.value+"_"+b.value+".txt", true);
        xhttp.send();
      }
      else {
        alert("Incorrect Password, try again.. you got this hacker !")
      }
    }
  </script>
```

h3ck3rBoi
ioBr3kc3h

54321@terceSrepuS

SuperSecret@12345

#### flag{edb0be532c540b1a150c3a7e85d2466e}