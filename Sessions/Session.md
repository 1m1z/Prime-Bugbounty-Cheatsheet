# Session’s

### Session Fixation Methodology

if you Do Logout and then you can access to your account from previous session or cookie in other browsers, its vulnerability

-if you clear cookies or jwt token or every token based authorization, if the request is still was loading its vulnerable.

### Session Expiration

test for expiration, capture a request and go for tomorrow, then resend the request and watch if you got 200 or 401 status code.

test for how many session you can open?

### weak session management

when you change your password in other sessions, the current session doesn't expired.

### Simultaneous Sessions

[https://github.com/OWASP/wstg/commit/6bac95a781ce98bb899cdf9831303ca536c0d113](https://github.com/OWASP/wstg/commit/6bac95a781ce98bb899cdf9831303ca536c0d113)