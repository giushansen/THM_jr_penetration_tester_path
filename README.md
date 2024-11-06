# THM Jr Penetration Tester


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


