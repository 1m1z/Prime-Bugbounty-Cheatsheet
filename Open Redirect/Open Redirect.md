# Open Redirect


tools: openredirectx 

web application enforce the user to go and load untrusted endpoint.

open redirect has two mode:

1-header based → usually leads to account take over and compile with ssrf.

2-javascript based → usually leads to XSS DOM based.

we have some redirect parameters for Hunt and testing:

- `redirect`
- `next`
- `url`
- `nexturl`
- `relay state`
- `forward`
    
    

### Tips:

- try to remove parameters like location or redirect in request and change status code to 200 and see what happend?

     - you can use DATA URL for bypassing ⇒ `?nexturl=data:text/html;base64,osidguheiughudnvu83whf88g=` 

→ base64 encode == attacker.com

-you can also haunt with google Dorks:

```
inurl:go
inurl:return
inurl:r_url
inurl:returnUrl
inurl:returnUri
inurl:locationUrl
inurl:goTo
inurl:return_url
```

- `redir`
- `goto`
- `site`
- `nextsite`
- `return`
- `getURLValue`

### The Methodology

1- after fuzzing all urls grab query based urls with qsreplace

2- text the result

---

1-set the auto repeater to replace all parameters with blab.com

2- set logger++ to find blab.com in urls and parameters