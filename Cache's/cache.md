# caches


tools: web cache deception scanner in burp extension

## Cache Deception

- try to logout and then click on previous page, if you are still logged in, its maybe vulnerable to this bug.
1. Frist Check Cacheing with non-exits static files in paths ([https://target.tld/profile/payments/nonexistent.css](https://target.tld/profile/payments/nonexistent.css))
2. If the response is not 404 and payments information is still displayed, the attack scenario is applicable

- try to set non-existent page or endpoint and if you got 200 status and page still load and you aint got and 404 not found, this may vulnerable to cache deception.
    - e.x: vallu.com/profile/dashboard/mine.php
    - e.x:vallu.com/profoile/dashboard/mine.php/1.js → if got 200 status or page loaded

## Cache Poisoning

- try to fill the inputs with payloads like xss then its maybe reflect or execute somewhere in application cause of caches bugs.