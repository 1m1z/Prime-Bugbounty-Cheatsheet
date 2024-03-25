# CSRF

Cross-Site Request Forgery(CSRF):
the CSRF force the target with a Forgery Request to Do some action that we want.

**first check?**

to doing SCRF attack we must check 2 things:

- request must be only SIMPLE
- the same-site parameter = none

> if you change the request method its called verb tampering
> 

> if you change the parameters of request like pass and user its called parameter tampering
> 

### Tips & Bypasses

- Remove CSRF TOKEN
- Weak or Guessable Token
- Try Verb Tampering
- Check referrer header
- Check for Reusable token
- Try remove main headers Like Referrer, Origin and Token Headers

### CSRF Protection:

- using CSRF Token in requests
- set a safe referrer header
- add one custom header to request so our request changes simple to preflight, so as you know when the request is preflight type, we cant exploit it and know it as a CSRF.
- add max-age header to the cookie
- secure header = HTTPOnly
- set the same site to **Strict**