# SSRF


tools: gopherus - qsreplace - httpx - waybackurl - ssrfmap - gau

a vulnerablility let an attacker forge the requests come from the server and control what postition or url handle the request.

and what make it vulnerable?

some paramerters like url and some methods (fopen).

we have 2 mode ssrf:

1-blown ssrf(full ssrf)

attacker → target server → internal network

2-blind ssrf:

attacker → target server → attacker server 

### How To Detect a SSRF?

full blown ssrf:

set value to local ip or host and watch it.

blind ssrf:

with out of band technique we must give our exchange server to the target to get request from target server 

 

### One thing Remeber:

schema = URI VS URL

URI Dosent have any port but the URL has the port so its know connect to who exaclty

the protocols of server we can use them for ssrf: 

- file*
- gopher*
- dict*
- http*
- https
- sftp
- ldap
- tftp

the best protocol you can use is gopher (if you found this open) why?

becuase the gopher connect via TCP (POST) and we can customize the packets like we want.

if the request is http, the http open coonection via GET and we cant do much in this protocol.

how to bypass the ssrf: use regex for filtring black and white list 

like: 

blacklist = file\:\/

whitelist = ^http?

- in some situation we can remove the protocol manually
    
    [file://etc/passwd](file://etc/passwd) → //etc/passwd
    
    the server put manually file protocol in our request.
    

### SSRF with whitelist-based input filters

- You can embed credentials in a URL before the hostname, using the `@` character. For example: `https://expected-host:fakepassword@evil-host`
- You can use the `#` character to indicate a URL fragment. For example: `https://evil-host#expected-host`
- You can leverage the DNS naming hierarchy to place
required input into a fully-qualified DNS name that you control. For
example: `https://expected-host.evil-host`
- You can URL-encode characters to confuse the URL-parsing code. This is particularly useful if the code that implements the
filter handles URL-encoded charact **SSRF with blacklist-based input filtersSSRF with blacklist-based input filtersers** differently than the code that
performs the back-end HTTP request. Note that you can also try [double-encoding](https://portswigger.net/web-security/essential-skills/obfuscating-attacks-using-encodings#obfuscation-via-double-url-encoding) characters; some servers recursively URL-decode the input they receive, which can lead to further discrepancies.
- You can use combinations of these techniques together.

** its harder bypassing via white list or allow list than block list**

you can use open redirect for this 

### **SSRF with blacklist-based input filters**

- Using an alternative IP representation of `127.0.0.1`, such as `2130706433`, `017700000001`, or `127.1`.
- Registering your own domain name that resolves to `127.0.0.1`. You can use `spoofed.burpcollaborator.net` for this purpose.
- Obfuscating blocked strings using URL encoding or case variation.
- Providing a URL that you control, which subsequently
redirects to the target URL. Try using different redirect codes, as well as different protocols for the target URL. For example, switching from
an `http:` to `https:` URL during the redirect has been shown to bypass some anti-SSRF filters.

### switching out encoding:

use hex,decimal and url encoding for bypass

use ipv6 for bypass 

use dns for bypass 

buy a domain 

create a dns property and give value a local ip like 127.0.0.1 

exmaple of bypass:

```objectivec
http://localhost:80%2523@stock.weliketoshop.net/admin/delete?username=carlos
```

*** if we found a open redirect vulnerability we can look for ssrf ***

** you can do port scanning via ssrf**

and look for time response and request code…

### The Methodology

gau → qsreplace → burp colab → httpx

stage 1 - give the url to gau or waybackurl

stage 2 - copy burp colabrator domain

stage 3 - replace all parameters and query strings with qsreplace

stage 4 - give the new edited file to httpx and check the burp colab to response given!