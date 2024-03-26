# 2FA Bypass or OTP Bypass


### **Status Code Manipulation**

- like changing 403 to 200…

### **Direct bypass**

you must know the direct URL first like Login and emm..

```
just try to access the next endpoint directly (you need to know the path of the next endpoint).
2) If this doesn't work, try to change the Referrer header as if you came from the 2FA page.

example :
site.com/login/otp_verification -> site.com/login/new_password
```

### **Referrer Check Bypass**

### **Developer’s Check (Check if bypass is in Clinet side)**

### **X-Forwarded-For**

```
add X-Forwarded-For: 127.0.0.1 in request
If it did not work try :
X-Originating-IP
X-Forwarded-Fo
X-Remote-IP
X-Remote-Addr
X-Client-IP
X-Host
X-Forwared-Host
```

### **Session permission**

### **Reusing token**

### **Reveal any kind of OTP codes in the response**

### **OTP bypass by Brute force (no Rate Limit)**

### **Bypass 2FA arbitrary input**

### **Change request method**