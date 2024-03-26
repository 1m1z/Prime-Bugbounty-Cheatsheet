# Security Misconfiguration & Information Disclosure

Tags: Data
Multi-select: ALL

## -  Information Disclosure

### Prevention (How To Prevent Information Disclosure)

use the tools that checks and create secrets storage system:

- https://github.com/hashicorp/vault
- https://github.com/duo-labs/secret-bridge

### Meta Data Disclosure

- pictures
- headers

### haunt for extension burpsuite Logger++:

`Response.Body == /(?i)([a-z0-9]+){0,}((_|-){0,}(\\s){0,})(APIkey|secret|accesstoken|apiToken|localhost)(\\s){0,}(=|:|is|>){1,}/ AND Response.Headers CONTAINS "application/javascript"` 

### Developer Comments

### Directory Listing

ffuf -u [https://target.com/FUZZ](https://target.com/FUZZ) -w fuzzurls.txt -mc 200

### TIPS

- search paste dump sites
- reconstruct source code and .git file
- use LinkFinder to find public link  and look for information disclosure

### Default Credentials

### Stack Trace - error Handling Check

### CORS Misconfiguration

### Verb Tampering

- use automated script:

`tamper(){
echo $1
for method in GET POST PUT DELETE HEAD OPTIONS; do
echo $method: $(curl -s -k $1 -X  $method -o /dev/null -w '%{http_code}) - %{size_download}')
done
}`

### Parameters Pollution

- test all parameters use autorepeater and logger++
- replace the qeury with first and last query
- `/search?q=Hello&q="><svg/onload=alert(1)>`  ⇒ Last query
- `q=hello&q=”><svg/onload=alert(1)>`  ⇒ First query
- *** all you need is do a thing that cause the web server make and error.***
- `id=100,101,blab...` ⇒ maybe make an error.
- e.g [site.com/admin](http://site.com/admin) 403 [site.com/admin](http://site.com/admin); 200
- id[]=10 id={10}

### Froce Browsing

- search on github and all search engine

### WEB Crawling

### RATE Limit Bypass

- use different ways
    - `sign-up -> Sign-up -> SignUp`
- using the bellow headers is cuaze make new IP and rollout the server:

X-Forwarded: 127.0.0.1
X-Forwarded-By: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Forwarded-For-Original: 127.0.0.1
X-Forwarded-For-Ip: 127.0.0.1
X-Forwarded-Host: 127.0.0.1
X-Forward-For: 127.0.0.1
Forwarded: 127.0.0.1
Forwarded-For: 127.0.0.1
Forwarded-For-Ip: 127.0.0.1
X-Originating-IP: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-Client-IP: 127.0.0.1
X-Host: 127.0.0.1
X-True-Ip: 127.0.0.1

### **Rate limit on OTP sms**

1. Capture the request.
2. Remove the country code +91 to [ ]
3. Modify the number from xxxxx-xxxxx to +91 xxxxx-xxxxx

### **Account Takeover**

always try logging with existing account and do something to takeover it.

try replace parameters like account ID and number and watch response.

###