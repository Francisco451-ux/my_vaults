liangqe7k2hrpjeplaphc7kmun

/items.php
/var/www/flag

```javascript
fetch("./students.json")
.then(response => {
   return response.json();
})
.then(jsondata => console.log(jsondata));
```