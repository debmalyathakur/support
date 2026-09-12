Support – TryHackMe Walkthrough
Overview

This repository contains a professional walkthrough of the "Support" room on TryHackMe. The objective of this challenge is to perform reconnaissance and web application enumeration, identify security weaknesses, exploit the discovered vulnerabilities, and retrieve the required flags.

The walkthrough documents the complete assessment process, including network reconnaissance, directory and endpoint enumeration, fuzzing, Local File Inclusion (LFI), Insecure Direct Object Reference (IDOR), brute-force testing, and exploitation of vulnerable application functionality.

Platform: TryHackMe
Category: Web Application Security
Difficulty: Easy–Medium
Primary Vulnerabilities: LFI, IDOR, Brute Force

Skills Demonstrated
Network Reconnaissance
Port & Service Enumeration
Web Application Enumeration
Directory Enumeration
Endpoint Discovery
Content Fuzzing
Nmap Scanning
Gobuster Enumeration
FFUF Fuzzing
Local File Inclusion (LFI)
Insecure Direct Object Reference (IDOR)
Brute-Force Attack Testing
HTTP Request Analysis
Parameter Manipulation
Vulnerability Identification
Exploitation & Post-Exploitation
Tools Used
Nmap
Gobuster
FFUF
Burp Suite
Browser Developer Tools
Linux Command Line
Wordlists
Walkthrough Contents

The walkthrough covers:

Initial target reconnaissance
Port and service enumeration using Nmap
Web application discovery
Directory and endpoint enumeration using Gobuster
Parameter and endpoint fuzzing using FFUF
Identifying a Local File Inclusion vulnerability
Exploiting LFI to access sensitive files
Identifying an IDOR vulnerability
Manipulating parameters and object references
Testing authentication functionality
Performing controlled brute-force testing
Obtaining valid access
Further enumeration after initial access
Retrieving the required flags
Reconnaissance

The assessment started with an Nmap scan to identify open ports and running services.

nmap -sC -sV <TARGET_IP>

The scan helped identify the attack surface and determine which services required further investigation.

Key Findings
Port	Service	Description
<PORT>	<SERVICE>	<DESCRIPTION>
<PORT>	<SERVICE>	<DESCRIPTION>
<PORT>	<SERVICE>	<DESCRIPTION>

Replace the placeholders with the exact results from your scan.

Web Enumeration

After identifying the web service, I performed directory and endpoint enumeration using Gobuster.

gobuster dir -u http://<TARGET_IP>/ -w /usr/share/wordlists/dirb/common.txt

This enumeration helped identify hidden directories, files, and potentially interesting application endpoints.

FFUF Enumeration

I then used FFUF for further endpoint and parameter discovery.

ffuf -u http://<TARGET_IP>/FUZZ -w /usr/share/wordlists/dirb/common.txt

Parameter fuzzing was also performed where required:

ffuf -u "http://<TARGET_IP>/<ENDPOINT>?<PARAM>=FUZZ" -w /usr/share/wordlists/dirb/common.txt

Form fuzzing for password of email

ffuf -u "http://<TARGET_IP>" -w /usr/share/wordlists/dirb/common.txt -X POST -H "Conten-type: form/....." -d "email=....&password=FUZZ" -fr "invalid cred"

The discovered endpoints were manually analyzed to identify potential attack vectors.

Vulnerability Analysis
1. Local File Inclusion (LFI)

During the web application assessment, I identified a parameter that appeared to reference files on the server.

Example:

http://<TARGET_IP>/<ENDPOINT>?file=<VALUE>

I tested whether the application was vulnerable to Local File Inclusion (LFI) by manipulating the file parameter.

Example payload:

../../../../etc/passwd

The successful retrieval of a local file confirmed the presence of an LFI vulnerability.

Impact

An LFI vulnerability may allow an attacker to read sensitive files from the server, potentially exposing:

System information
Application configuration
Credentials
Source code
Environment information
Other sensitive files
2. IDOR

During application analysis, I identified functionality where object references were controlled through request parameters.

For example:

?id=1

I tested whether changing the object identifier allowed access to resources belonging to another user.

Example:

?id=2

The application returned information that should not have been accessible to the current user.

This confirmed an Insecure Direct Object Reference (IDOR) vulnerability.

Impact

IDOR can allow an attacker to access or modify unauthorized resources by simply changing an identifier in the request.

3. Brute-Force Vulnerability

The authentication functionality was tested for insufficient brute-force protection.

The testing involved identifying the relevant authentication endpoint and sending multiple controlled authentication attempts.

Example using FFUF:

ffuf -u http://<TARGET_IP>/<LOGIN_ENDPOINT> \
-X POST \
-d "username=<USERNAME>&password=FUZZ" \
-w <WORDLIST> \
-H "Content-Type: application/x-www-form-urlencoded"

The responses were compared to identify differences between valid and invalid authentication attempts.

Security Weakness

The application lacked sufficient protection against repeated authentication attempts.

Potential mitigations include:

Rate limiting
Account lockout
CAPTCHA
MFA
Login throttling
Monitoring and alerting
Exploitation

After identifying the vulnerabilities, I chained the discovered information and functionality to progress through the room.

The overall attack path was:

Nmap
  ↓
Service Enumeration
  ↓
Web Application Discovery
  ↓
Gobuster
  ↓
FFUF
  ↓
LFI
  ↓
Sensitive Information Discovery
  ↓
IDOR
  ↓
Brute-Force Testing
  ↓
Valid Access
  ↓
Flag Retrieval
Proof of Concept

The main vulnerabilities identified during the assessment were:

Vulnerability	Description	Impact
LFI	Local files could be included/read through a vulnerable parameter	Sensitive file disclosure
IDOR	Object identifiers could be manipulated to access unauthorized resources	Unauthorized information access
Brute Force	Authentication endpoint allowed repeated login attempts	Potential account compromise
Flags
User Flag
THM{REDACTED}
Root / Final Flag
THM{REDACTED}

Flags have been redacted to avoid unnecessarily spoiling the TryHackMe room.

Key Takeaways

This room helped strengthen my practical understanding of:

Network reconnaissance
Nmap service enumeration
Web application enumeration
Directory discovery
FFUF-based fuzzing
Gobuster enumeration
Local File Inclusion
Insecure Direct Object References
Authentication security
Brute-force testing
HTTP request manipulation
Vulnerability chaining
Practical penetration-testing methodology
Lessons Learned

The main lesson from this room was that vulnerabilities should not always be analyzed in isolation.

Information discovered during reconnaissance and enumeration can often be combined with application-level vulnerabilities to create a practical attack path.

This room provided hands-on experience with identifying vulnerabilities, validating their impact, and using the discovered information to progress through a controlled penetration-testing environment.

Disclaimer

This walkthrough is intended solely for educational purposes within the TryHackMe platform.

Do not attempt these techniques against systems that you do not own or have explicit permission to test.

Author

Debmalya Thakur

Junior System Administrator | Aspiring Penetration Tester | VAPT Enthusiast

GitHub: https://github.com/debmalyathakur

LinkedIn: https://linkedin.com/in/

License

This project is provided for educational and learning purposes only.
