# Senstivity Data Discovery

- New endpoints, one time i found a whole list of endpoints in the comments
- Hidden parameters
- API keys, sometimes they are supposed to be public though, so be careful with these. Verify the impact before you report! [https://github.com/streaak/keyhacks](https://github.com/streaak/keyhacks)
- Business logic, which we might be able to abuse like client side calculations of prizes
- Secrets/passwords
- Potentially dangerous areas in the javascript code such as eval() or setinnerhtml(). These are DOM sinks and can lead to DOM XSS

burp extentions

retire.js

dynamic js

sentivity data discoverer

js link finder

json web token

logger++ filters

truffle hug

use usefull regex

[GitHub - l4yton/RegHex: A collection of regexes for every possbile use](https://github.com/l4yton/RegHex?source=post_page-----c95a8aa7037a--------------------------------)

[Analyzing JavaScript Files To Find Bugs](https://realm3ter.medium.com/analyzing-javascript-files-to-find-bugs-820167476ffe)

`cat domains.txt | katana | grep js | httpx -mc 200 | tee js.txt`

1. `cat domains.txt | katana`: This command uses the `cat` utility to display the contents of the file `domains.txt`. It assumes that `domains.txt` contains a list of domain names or URLs and paths by | to katana to crawl urls from domains
2. `grep js`: The `grep` command is used for pattern matching in text files. In this case, it is searching for lines that contain the ".js" pattern, which indicates
JavaScript files. This filters the output only to include lines that
mention JavaScript files.
3. `httpx -mc 200`: This command utilizes the `httpx` tool to send HTTP requests and retrieve responses from the filtered URLs. The `mc 200` option only shows URLs that return a successful HTTP status code of 200 (OK). This filters out URLs that do not exist or return errors.
4. `tee js.txt`: The `tee` command is used to display the output of a command and save it to a
file simultaneously. In this case, it saves the filtered URLs that match the previous criteria into a file called `js.txt`.

`nuclei -l js.txt -t ~/nuclei-templates/exposures/ -o js_bugs.txt`

Download All links in js.txt

and do search about these

code :

```
file="js.txt"

# Loop through each line in the file
while IFS= read -r link
do
    # Download the JavaScript file using wget
    wget "$link"
done < "$file"
```

```
grep -r -E "aws_access_key|aws_secret_key|api key|passwd|pwd|heroku|slack|firebase|swag
```

. To do this, we can use a tool by GitHub repo, [Bug-Bounty-Toolz](https://github.com/m4ll0k/Bug-Bounty-Toolz), which has a simple python script [getjswords.py](https://github.com/m4ll0k/Bug-Bounty-Toolz/blob/master/getjswords.py).
 This goes through all the JavaScript files that were captured using the
 ScriptHunter tool in the previous step and creates a wordlist 
containing all the information that is in the JavaScript files.

[](https://github.com/m4ll0k/BBTz/blob/master/getjswords.py)

using developer tool to find bugs - expetions - F12

[How I made $10K in bug bounties from GitHub secret leaks](https://tillsongalloway.com/finding-sensitive-information-on-github/index.html)