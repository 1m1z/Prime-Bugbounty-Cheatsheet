# Reset Password


### User manipulation via Password Reset

- you can  set username to password reset page and check if user is exist → User Enumeration

### **Password reset link not expiring**

- check and change token and check time and watch if link expired or not.

### **Password reset token leak via referrer**

### **Password reset token leak via response**

### **Host Header Poisoning**

```

Add X-Forwarded-Host header :

Host: attacker.com
X-Forwarded-Host: target.com

or :

Host: bing.com
X-Forwarded-Host: target.com

or :
Host: target.com
Host: attacker.com
```

### **No rate limiting on password reset**

`1) Start the burp suite and intercept the password reset request
2) Send to intruder
3) Use null payload`

### **HTML Injection in Password reset page**

- see [https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/tree/main/Reset Password vulnerabilities](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/tree/main/Reset%20Password%20vulnerabilities)

# Methodology+ (Pentest)

- make sure your change password session is have expire time, if its not Report in Pentest.
- make sure the reset password progress is with 2FA mechanism.

## password overwrite

look forward to finding can you find out how reset password link is working (email) and can you manipulate link?

`http://site.ir/account/forgotPassword/chooseNewPassword/**?SECRETID=23irj3it3o**`

the link give SECRETID each time we have click on reset password

if we look at the profile request we find each user have access key like SERVERID

`user=username&public_key=b55d4d&private_key=0FEBF3&a5a06f09cc4afe95f8b314e63cf7f74399a2d7ab7e39d31fe90accee5d22=**634346c2cbe8e242d52f1443a8511f51211111c9a4f8b384945ce5d4c92c**`

and what if we replace the Access key to that Secret ID in email?

like this:

`http://vulnerable-target/account/forgotPassword/chooseNewPassword/**?SECRETID=634346c2cbe8e242d52f1443a8511f51211111c9a4f8b384945ce5d4c92c**`

Should You better Read this repo Also:

[https://github.com/Az0x7/vulnerability-Checklist/blob/main/reset password/reset_password_checklist.md](https://github.com/Az0x7/vulnerability-Checklist/blob/main/reset%20password/reset_password_checklist.md)