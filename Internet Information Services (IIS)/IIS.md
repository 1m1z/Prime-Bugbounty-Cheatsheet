# IIS

Tags: web server

### Resources

[https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/iis-internet-information-services](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/iis-internet-information-services) → IIS 

[https://book.hacktricks.xyz/pentesting-web/deserialization/exploiting-__viewstate-parameter](https://book.hacktricks.xyz/pentesting-web/deserialization/exploiting-__viewstate-parameter)

[https://blog.mindedsecurity.com/2018/10/from-path-traversal-to-source-code-in.html](https://blog.mindedsecurity.com/2018/10/from-path-traversal-to-source-code-in.html) → Stateview attack

---

### The Methodology

1. Scan with authomated tools

---

# ATTACKS

### Server Finger Print

try to looking for 

try to find an 404 page or auth/errorfe.aspx to seeing this result in response

- X-Owa-Version
- X-Feserver

### Server Finger Print 2

look for:

try to find an 404 page or auth/errorfe.aspx to seeing this result in response and **change method to POST**

- server: Microsoft-IIS/10.0
- Server: Microsoft-HTTPAPI/2.0

### Web application Finger Print

look for:

- X-Aspnet-Version: 4.*x
- X-Powered-By: ASP.NET

### Host Header Injection

in important route like /owa or /auth 

change the Host header value to whatever you want like Host: blab.ir

so you see the site redirect to blab.ir

### Hidden NTLM Attack

and use Sns tool

first of all try to find an page that return **401 or unuthenticated**

- insert that paylaod in request
- Authorization: NTLM TlRMTVNTUAABAAAAB4IIAAAAAAAAAAAAAAAAAAAAAAA=

![https://miro.medium.com/v2/resize:fit:1100/format:webp/0*gx4jHrhFvw5Igh_Q.png](https://miro.medium.com/v2/resize:fit:1100/format:webp/0*gx4jHrhFvw5Igh_Q.png)

- if that server is vulnerable return header with hashed value like:
    - WWW-Authenticate
- then you must install **NTLM Challenge Decoder** extention for burp
- then in response you can see **SSP Decode**r tab and that decode the NTLM Encreyption

**** try to reach */Authodiscover/Authodiscover.xml page for this bug** 

**and then change method to DEBUG or SOAP to reach 401 error******

[Internal Information Disclosure using Hidden NTLM Authentication | by Mike Brown | The Startup | Medium](https://medium.com/swlh/internal-information-disclosure-using-hidden-ntlm-authentication-18de17675666)

### Internal IP Disclosure

1. try to reach a 302 redirect page or request 
2. change HTTP/2.0 to HTTP/1.1
3. remove Hots: *.* header then follow redirection
4. you can see the internal ip disclosure in response or request.

### CVE’s

- MS - IIS/10
    
    ms - exchange server 019 cu12 mar - 15.01.1118.020 - ta 26 -> assigned CVE -> 2022-41040 (exchange server)
    [https://msrc.microsoft.com/update-guide/vulnerability/CVE-2022-41040](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2022-41040) -> for information and publicy
    
    OWA - 26
    "15.2.1118.26": {
    "cpe": "cpe:/a:microsoft:exchange_server:2019:cumulative_update_12:*:*:*:*:*:*",
    "cves": [
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-28310",
    "last-modified": "2023-06-22T12:55:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    [https://www.cve.org/CVERecord?id=CVE-2023-28310](https://www.cve.org/CVERecord?id=CVE-2023-28310)[https://msrc.microsoft.com/update-guide/vulnerability/CVE-2023-28310](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2023-28310)
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-32031",
    "last-modified": "2023-06-22T13:32:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-38185",
    "last-modified": "2023-08-10T21:15:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-38182",
    "last-modified": "2023-08-11T14:27:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-35388",
    "last-modified": "2023-08-11T15:56:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-35368",
    "last-modified": "2023-08-11T15:58:00",
    "summary": "Microsoft Exchange Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-38181",
    "last-modified": "2023-08-11T15:00:00",
    "summary": "Microsoft Exchange Server Spoofing Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-36744",
    "last-modified": "2023-09-15T16:30:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-36756",
    "last-modified": "2023-09-15T14:15:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-36745",
    "last-modified": "2023-09-15T16:28:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-36757",
    "last-modified": "2023-09-14T22:37:00",
    "summary": "Microsoft Exchange Server Spoofing Vulnerability"
    }
    ]
    },
    
    "15.2.1118.20": {
    "cpe": "cpe:/a:microsoft:exchange_server:2019:cumulative_update_12:*:*:*:*:*:*",
    "cves": [
    {
    "cvss": 7.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-21764",
    "last-modified": "2023-04-27T19:15:00",
    "summary": "Microsoft Exchange Server Elevation of Privilege Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-21762",
    "last-modified": "2023-04-27T19:15:00",
    "summary": "Microsoft Exchange Server Spoofing Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-21745",
    "last-modified": "2023-04-27T19:15:00",
    "summary": "Microsoft Exchange Server Spoofing Vulnerability"
    },
    {
    "cvss": 7.5,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-21761",
    "last-modified": "2023-04-27T19:15:00",
    "summary": "Microsoft Exchange Server Information Disclosure Vulnerability"
    },
    {
    "cvss": 7.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-21763",
    "last-modified": "2023-04-27T19:15:00",
    "summary": "Microsoft Exchange Server Elevation of Privilege Vulnerability"
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-21706",
    "last-modified": "2023-02-23T15:56:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-21529",
    "last-modified": "2023-02-22T17:26:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-21707",
    "last-modified": "2023-02-23T16:03:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 7.2,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-21710",
    "last-modified": "2023-02-23T16:03:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-28310",
    "last-modified": "2023-06-22T12:55:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-32031",
    "last-modified": "2023-06-22T13:32:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-38185",
    "last-modified": "2023-08-10T21:15:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-38182",
    "last-modified": "2023-08-11T14:27:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-35388",
    "last-modified": "2023-08-11T15:56:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-35368",
    "last-modified": "2023-08-11T15:58:00",
    "summary": "Microsoft Exchange Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.8,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-38181",
    "last-modified": "2023-08-11T15:00:00",
    "summary": "Microsoft Exchange Server Spoofing Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-36744",
    "last-modified": "2023-09-15T16:30:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-36756",
    "last-modified": "2023-09-15T14:15:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-36745",
    "last-modified": "2023-09-15T16:28:00",
    "summary": "Microsoft Exchange Server Remote Code Execution Vulnerability"
    },
    {
    "cvss": 8.0,
    "cvss-time": null,
    "cwe": "NVD-CWE-noinfo",
    "id": "CVE-2023-36757",
    "last-modified": "2023-09-14T22:37:00",
    "summary": "Microsoft Exchange Server Spoofing Vulnerability"
    }
    ]
    },
    

### IIS - TILDE Scan

~ == tilde

you must scan to reach defult urls

[https://github.com/irsdl/IIS-ShortName-Scanner](https://github.com/irsdl/IIS-ShortName-Scanner)

### Rate Limit On Login Exchange Page

### Telerik web-UI

[https://github.com/bao7uo/dp_crypto](https://github.com/bao7uo/dp_crypto)

### View States

Hackvertor burp extention

[Exploiting Deserialisation in ASP.NET via ViewState](https://soroush.me/blog/2019/04/exploiting-deserialisation-in-asp-net-via-viewstate/)

[Exploiting __VIEWSTATE without knowing the secrets](https://book.hacktricks.xyz/pentesting-web/deserialization/exploiting-__viewstate-parameter)

### Default passwords

### Debug Mode - DEBUG Method

reach the page called `trace.axd` 

change method to DEBUG 

if debug mode was enabled that show sentivity datas

---

### Exploits

[https://github.com/righel/ms-exchange-version-nse](https://github.com/righel/ms-exchange-version-nse)

usually the exploits is different for each IIS-version or exchange version 

to version discovery:
`nmap -p 443 --script ms-exchange-version.nse <target>`

to CVE discovery:
`nmap -p 443 --script ms-exchange-version.nse --script-args=showcves <target>`

to CPE discovery:
`nmap -p 443 --script ms-exchange-version.nse --script-args=showcpe <target>`

and you can use searchsploit tool

`searchsploit Microsoft IIS/10`

### check for log.txt

- Elmah.axd
- Errorlog.axd