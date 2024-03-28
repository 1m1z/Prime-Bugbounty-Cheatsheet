# JWT


tools:

Json Web Tocken burp extention → to decoding JWT

JWTEditor burp extention → to seeing live request who use JWT

[https://jwt.io/](https://jwt.io/)

[https://www.jstoolset.com/jwt](https://www.jstoolset.com/jwt) → gives us more information 

[https://token.dev/](https://token.dev/)

![Untitled](Untitled.png)

**Json Web Token** 

- JWT has 3 part
    1. the header: usaully has 2 part 
        - the alg and typ
        
        ```
        {
            "alg": "HS256",
            "typ": "JWT"
        }
        ```
        
    2. the payload: used to base of Authentication
        
        ```
        {
            "kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
            "typ": "JWT",
            "alg": "RS256",
            "jwk": {
                "kty": "RSA",
                "e": "AQAB",
                "kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
                "n": "yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9m"
            }
        }
        ```
        
    3. the signature: the last text in paylaod is signature key
        
        ```python
        Signature = "i4Z5xurxMWsxWrzKiCYUXxAT3ViSgyx2IUuEO1H8N9sdxkc5JUcnCFpixhLlfW6ALcMrGXiDqKjCcgsFATmyfdEMqtv08Y3kK8JYqWq0e4n2ba6Zd3f2x3Gj-dBdXO4NNbL9FmUUVY64FV_dCMuUZ1BBfzNtPMMw_GsP5sSSBZ2h1dh0B_owRI82NGSDYEvUMpTtV4_VN9S9b3Na7tAKBDL_isf9xUqv7cEvOEX41hhxPKUztLs4LSTF5ef6EGDFHYKlY5TUJk2L5f0D1lDbTO01bTYiG4QayIOc4xQkBjEvtHKJ1uMOzWOe7o2YzllW_cBbPrdlULn4HW2gkT_QRDSoxDoxzmvvJEFZOzcP4z60KIcy1rmKoUf7MOBBfAaEykDCPzvrE9puRPTStnNPCkV52CYUv_HHc31gvdT3k9pPn5ka1zyZc1p2wy-CTWpABDe0MhP6fBubhlZXIKkhRW3Ca7DviQhvOyQrB73Q4viglG7cg_yU9fEPmWEtU-Lb6n75-8__NDk7QjZaUIJLXElDX9WP9ApmOkvIu8gNuGmIaljuBkA87ewD5OzswZt-5N441rvWowHbaPyIc3eQV7Ol901AeY0xUTTiNFUgzLClpcrszNMi7b4wZ3b2NIMlUkhivZ3l7PdEVMI6lYYKJScHam92KmPUMqiZ9O0LZrU"
        ```
        
    
    ### Using with Automated jwt_tool and Fuzzing
    
    [https://github.com/ticarpi/jwt_tool](https://github.com/ticarpi/jwt_tool)
    
    the command:
    
    ```python
    python3 jwt_tool.py -M at -t "[https://api.azkivam.com/requests/982045](https://api.azkivam.com/requests/982045)" -rh "Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJBY2Nlc3NUb2tlbiIsImlzcyI6ImlyLmF6a2kubG9hbiIsImV4cCI6MTY5NDUwOTQyMSwidXNlcklkIjo4MzU4MTUsImlhdCI6MTY5NDUwMjIyMX0.H2SzpYwh71BAJ8s7eotqJLnZLFWhnnsp3dkgVUke_3R63LyX5HS-4zPMJWFbWE6PfwtAUddKpFy5PaEGnbhriA"
    ```
    
    ---
    
    [RFC 7515: JSON Web Signature (JWS)](https://datatracker.ietf.org/doc/html/rfc7515#section-3)
    
    # ** Check This PDF **
    
    [20190222--Nyffenegger-JWAT.pdf](20190222--Nyffenegger-JWAT.pdf)
    
    ---
    
    ### Typical JWT Attacks
    
    - CVE-2018-0114
    - Algoritm Confiuhsion - Signeture Tampring
    - None Signeture
    - Kid Injection
    - Trivial secret