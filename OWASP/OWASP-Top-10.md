# 🛡️ OWASP Top 10 - Web Application Security Risks

> The most critical security risks to web applications (2021 Edition)

---

## 📋 Overview

The **OWASP Top 10** is a standard awareness document for developers and web application security. It represents a broad consensus about the most critical security risks to web applications. Understanding these vulnerabilities is essential for anyone in cybersecurity.

**Why OWASP Top 10 Matters:**
- 🎯 Industry standard for web security
- 📊 Based on real-world data and expert consensus
- 🔍 Essential knowledge for penetration testers
- 💼 Required for bug bounty hunters
- 🛡️ Guides secure development practices

---

## 🔟 OWASP Top 10 (2021)

### A01:2021 - Broken Access Control

**Description**: Restrictions on what authenticated users can do are often not properly enforced. Attackers can exploit these flaws to access unauthorized functionality and/or data.

**Examples:**
- Accessing other users' accounts by changing URL parameters
- Viewing or editing someone else's account
- Acting as an admin when logged in as a regular user
- Manipulating metadata like JWT tokens

**Common Vulnerabilities:**
```
- Insecure Direct Object References (IDOR)
- Missing function-level access control
- Metadata manipulation (JWT, cookies)
- CORS misconfiguration
- Force browsing to authenticated pages
```

**Testing Techniques:**
```bash
# Test parameter manipulation
http://site.com/profile?user_id=123  # Change to different user_id

# Test path traversal
http://site.com/../../admin/

# Test for forced browsing
http://site.com/admin/
http://site.com/api/admin/users

# JWT manipulation
# Decode, modify, and re-encode JWT tokens
```

**Prevention:**
- Implement proper access control checks
- Deny by default
- Use centralized access control mechanisms
- Log access control failures
- Rate limit API access

---

### A02:2021 - Cryptographic Failures

**Description**: Failures related to cryptography (previously known as Sensitive Data Exposure) which often lead to exposure of sensitive data.

**Examples:**
- Transmitting data in clear text (HTTP instead of HTTPS)
- Using old or weak cryptographic algorithms
- Default, weak, or hard-coded passwords
- Missing or improper certificate validation

**Common Issues:**
```
- No encryption for data in transit
- Weak encryption algorithms (MD5, SHA1)
- Improper key management
- No encryption for sensitive data at rest
- Using deprecated protocols (TLS 1.0, SSL)
```

**Testing Techniques:**
```bash
# Check SSL/TLS configuration
nmap --script ssl-enum-ciphers -p 443 target.com
testssl.sh https://target.com

# Check for sensitive data exposure
curl -v http://site.com/api/users | grep -i "password\|ssn\|credit"

# Check for weak hashing
# Look for MD5, SHA1 in password storage
```

**Prevention:**
- Encrypt all sensitive data at rest and in transit
- Use strong, up-to-date encryption algorithms
- Proper key management
- Disable caching for sensitive data
- Use HTTPS everywhere (HSTS)

---

### A03:2021 - Injection

**Description**: Injection flaws occur when untrusted data is sent to an interpreter as part of a command or query. The attacker's hostile data tricks the interpreter into executing unintended commands.

**Types of Injection:**
```
1. SQL Injection (SQLi)
2. NoSQL Injection
3. Command Injection (OS)
4. LDAP Injection
5. XPath Injection
6. XML Injection
7. Expression Language Injection
```

**SQL Injection Examples:**
```sql
-- Authentication bypass
' OR '1'='1' --
admin' --

-- Union-based SQLi
' UNION SELECT null, username, password FROM users--

-- Time-based blind SQLi
' AND SLEEP(5)--

-- Error-based SQLi
' AND 1=CONVERT(int, (SELECT @@version))--
```

**Command Injection Examples:**
```bash
# Basic command injection
; ls -la
| whoami
& cat /etc/passwd

# With URL encoding
%3B+cat+%2Fetc%2Fpasswd

# Time-based detection
; sleep 10
& ping -c 10 127.0.0.1
```

**Testing Techniques:**
```bash
# SQLMap for SQL injection
sqlmap -u "http://site.com/page?id=1" --batch --dbs

# Manual SQL injection testing
' OR 1=1--
' OR 'a'='a
1' AND '1'='1

# Command injection testing
; whoami
| whoami
`whoami`
$(whoami)
```

**Prevention:**
- Use parameterized queries (prepared statements)
- Input validation and sanitization
- Principle of least privilege for DB accounts
- Use ORM frameworks safely
- Escape special characters

---

### A04:2021 - Insecure Design

**Description**: New category focusing on risks related to design and architectural flaws. Missing or ineffective control design.

**Examples:**
- Missing rate limiting allowing credential stuffing
- Trust boundary violations
- Insecure workflows
- Missing or improper threat modeling

**Common Issues:**
```
- No rate limiting on critical functions
- Missing security controls by design
- Improper business logic
- Lack of segregation of tenant data
- Missing threat modeling in SDLC
```

**Testing Techniques:**
```bash
# Test rate limiting
for i in {1..1000}; do 
    curl -X POST http://site.com/api/login \
    -d "username=admin&password=test$i"
done

# Test business logic
# Try purchasing items with negative price
# Try applying discounts multiple times
# Test for race conditions
```

**Prevention:**
- Implement secure development lifecycle (SDLC)
- Use threat modeling
- Integrate security into every development phase
- Implement proper rate limiting
- Use secure design patterns and libraries

---

### A05:2021 - Security Misconfiguration

**Description**: Security misconfiguration is the most commonly seen issue. This is often a result of insecure default configurations, incomplete configurations, open cloud storage, misconfigured HTTP headers, and verbose error messages.

**Examples:**
- Default accounts with default passwords
- Unnecessary features enabled
- Directory listing enabled
- Detailed error messages revealing system information
- Missing security headers

**Common Misconfigurations:**
```
- Unpatched systems
- Unused pages, features enabled
- Default accounts active
- Misconfigured permissions
- Missing security headers
- Verbose error messages
- Open cloud storage buckets
```

**Testing Techniques:**
```bash
# Check for default credentials
hydra -L usernames.txt -P passwords.txt site.com http-post-form

# Check security headers
curl -I https://site.com | grep -i "x-frame\|x-xss\|strict-transport"

# Check for directory listing
curl http://site.com/uploads/

# Check for information disclosure
curl http://site.com/test.php | grep -i "error\|warning\|mysql"

# Cloud storage enumeration
aws s3 ls s3://bucket-name --no-sign-request
```

**Prevention:**
- Implement minimal platform with only necessary features
- Review and update configurations regularly
- Use automated security configuration scanning
- Implement proper security headers
- Disable directory listing and verbose errors
- Keep systems updated and patched

---

### A06:2021 - Vulnerable and Outdated Components

**Description**: Components run with the same privileges as the application itself. If a vulnerable component is exploited, such an attack can facilitate serious data loss or server takeover.

**Examples:**
- Using libraries with known vulnerabilities
- Outdated frameworks and platforms
- Unnecessary dependencies
- Not regularly patching systems

**Common Issues:**
```
- Outdated CMS (WordPress, Joomla, Drupal)
- Vulnerable JavaScript libraries
- Outdated server software
- End-of-life components
- Unpatched operating systems
```

**Testing Techniques:**
```bash
# Identify technologies
whatweb https://site.com
wappalyzer https://site.com

# Check for known vulnerabilities
nuclei -u https://site.com -t cves/

# Scan for vulnerable components
retire.js --path /path/to/webroot
npm audit
snyk test

# Nmap with version detection
nmap -sV -p- target.com
```

**Prevention:**
- Maintain inventory of all components
- Monitor security bulletins
- Remove unused dependencies
- Use automated vulnerability scanning
- Only obtain components from official sources
- Use tools like Dependabot, Snyk

---

### A07:2021 - Identification and Authentication Failures

**Description**: Confirmation of the user's identity, authentication, and session management is critical. Authentication failures can allow attackers to compromise passwords, keys, or session tokens.

**Examples:**
- Weak password requirements
- No rate limiting on authentication
- Session fixation attacks
- Missing multi-factor authentication
- Exposing session IDs in URLs

**Common Vulnerabilities:**
```
- Credential stuffing attacks
- Brute force attacks
- Weak password policies
- Default credentials
- Insecure session management
- Missing MFA
- Predictable session tokens
```

**Testing Techniques:**
```bash
# Brute force authentication
hydra -l admin -P /usr/share/wordlists/rockyou.txt site.com http-post-form

# Test for username enumeration
# Different responses for valid/invalid users

# Session testing
# Check session timeout
# Test for session fixation
# Check for secure and httponly flags on cookies

# Test MFA bypass
# Try authentication with only password
```

**Prevention:**
- Implement multi-factor authentication
- Strong password policies
- Rate limiting and account lockout
- Secure session management
- No default credentials
- Use secure session storage
- Implement proper logout functionality

---

### A08:2021 - Software and Data Integrity Failures

**Description**: New category focusing on making assumptions related to software updates, critical data, and CI/CD pipelines without verifying integrity.

**Examples:**
- Insecure deserialization
- Using CDN or libraries from untrusted sources
- Auto-update functionality without integrity verification
- Unsigned or unverified serialized objects

**Common Issues:**
```
- Insecure deserialization
- Using unverified dependencies
- Lack of code signing
- Insecure CI/CD pipelines
- Tampering with update mechanisms
```

**Testing Techniques:**
```bash
# Check for insecure deserialization
# Send serialized objects with malicious payloads

# Verify integrity of external resources
# Check for SRI (Subresource Integrity) tags

# CI/CD testing
# Check for exposed CI/CD credentials
# Test for pipeline injection
```

**Prevention:**
- Use digital signatures to verify integrity
- Ensure libraries and dependencies are from trusted sources
- Use Subresource Integrity (SRI)
- Review code and configuration changes
- Segregate CI/CD environments
- Use dependency checking tools

---

### A09:2021 - Security Logging and Monitoring Failures

**Description**: Insufficient logging and monitoring, coupled with missing or ineffective integration with incident response, allows attackers to further attack systems, maintain persistence, and pivot to more systems.

**Examples:**
- Not logging authentication failures
- Logs not being monitored
- Logs stored only locally
- Inadequate alerting thresholds
- Penetration tests and scans not triggering alerts

**Common Issues:**
```
- No logging of security events
- Logs not monitored
- Missing audit logs
- Log integrity not protected
- No incident response plan
- Insufficient visibility
```

**Testing Techniques:**
```bash
# Check what gets logged
# Perform various attacks and check logs

# Test for log injection
# Try injecting newlines and fake log entries

# Check log access controls
# Who can access logs?

# Verify alerting
# Do obvious attacks trigger alerts?
```

**Prevention:**
- Log all authentication and access control events
- Ensure logs are in a format consumable by log management
- High-value transactions have audit trail with integrity
- Effective monitoring and alerting
- Establish incident response plan
- Use SIEM systems

---

### A10:2021 - Server-Side Request Forgery (SSRF)

**Description**: SSRF flaws occur whenever a web application is fetching a remote resource without validating the user-supplied URL. It allows an attacker to coerce the application to send a crafted request to an unexpected destination.

**Examples:**
- Accessing internal services
- Port scanning internal network
- Reading cloud metadata
- Bypassing firewalls
- Exploiting trust relationships

**Common Attacks:**
```
- Cloud metadata access (AWS, Azure, GCP)
- Internal service enumeration
- Port scanning
- Local file reading
- Bypassing authentication
```

**Testing Techniques:**
```bash
# Basic SSRF test
url=http://localhost:80
url=http://127.0.0.1:80
url=http://169.254.169.254/latest/meta-data/  # AWS metadata

# Internal network scanning
url=http://192.168.1.1:80
url=http://10.0.0.1:22

# Protocol manipulation
url=file:///etc/passwd
url=gopher://internal-host:6379/_SET%20key%20value

# DNS rebinding
url=http://attacker-controlled-domain.com
```

**Prevention:**
- Sanitize and validate all user-supplied input
- Enforce URL schema, port, and destination whitelist
- Disable HTTP redirections
- Use network segmentation
- Implement rate limiting
- Don't send raw responses to clients

---

## 🎯 Testing Methodology

### 1. Information Gathering
```bash
# Technology detection
whatweb https://target.com
nmap -sV target.com

# Directory enumeration
gobuster dir -u https://target.com -w wordlist.txt

# Subdomain enumeration
sublist3r -d target.com
```

### 2. Vulnerability Assessment
```bash
# Automated scanning
nuclei -u https://target.com -t cves/
nikto -h https://target.com

# Manual testing with Burp Suite
# Proxy all traffic through Burp
# Analyze requests and responses
# Test for OWASP Top 10
```

### 3. Exploitation
```bash
# SQL Injection
sqlmap -u "https://target.com/page?id=1"

# XSS Testing
# Insert payloads in all input fields

# Authentication testing
hydra -L users.txt -P pass.txt target.com http-post-form
```

---

## 📖 Learning Resources

### Hands-On Practice
- **PortSwigger Academy**: [https://portswigger.net/web-security](https://portswigger.net/web-security)
- **OWASP WebGoat**: [https://owasp.org/www-project-webgoat/](https://owasp.org/www-project-webgoat/)
- **TryHackMe**: OWASP Top 10 room
- **HackTheBox**: Web challenges

### Documentation
- **OWASP Top 10 Official**: [https://owasp.org/Top10/](https://owasp.org/Top10/)
- **OWASP Testing Guide**: [https://owasp.org/www-project-web-security-testing-guide/](https://owasp.org/www-project-web-security-testing-guide/)
- **OWASP Cheat Sheets**: [https://cheatsheetseries.owasp.org/](https://cheatsheetseries.owasp.org/)

### Video Training
- **OWASP Top 10 Course** - Various platforms
- **Bug Bounty Hunter** - Real-world examples
- **Cybrary** - Web application security

---

## 🔗 Tools for Testing OWASP Top 10

| Tool | Use Case |
|------|----------|
| Burp Suite | Manual testing, all categories |
| OWASP ZAP | Automated scanning |
| SQLMap | SQL injection |
| XSStrike | XSS detection |
| Nikto | Misconfiguration detection |
| Nuclei | CVE scanning |
| Nmap | Information gathering |
| Gobuster | Directory enumeration |

---

## ✅ Security Checklist

- [ ] Test for broken access control
- [ ] Verify encryption implementation
- [ ] Test all input fields for injection
- [ ] Review application design and architecture
- [ ] Audit configurations and security settings
- [ ] Check for outdated components
- [ ] Test authentication mechanisms
- [ ] Verify software/data integrity
- [ ] Review logging and monitoring
- [ ] Test for SSRF vulnerabilities

---

*Understanding OWASP Top 10 is the foundation of web application security. Test ethically and report responsibly.*
