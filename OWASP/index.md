---
title: "OWASP Top 10 - Web Application Security Risks"
permalink: /OWASP/
layout: single
author_profile: true
toc: true
toc_sticky: true
---

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

### A01:2021 - Broken Access Control ⚠️

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
```

**Prevention:**
- Implement proper access control checks
- Deny by default
- Use centralized access control mechanisms
- Log access control failures
- Invalidate tokens on logout

---

### A02:2021 - Cryptographic Failures 🔐

**Description**: Previously known as "Sensitive Data Exposure." Failures related to cryptography (or lack thereof) that often lead to exposure of sensitive data.

**Common Issues:**
- Transmitting data in clear text (HTTP instead of HTTPS)
- Using weak/old cryptographic algorithms
- Poor key management
- Not using encryption for sensitive data
- Weak password hashing

**Testing:**
```bash
# Check for HTTP usage
curl -I http://example.com

# Check SSL/TLS configuration
sslscan example.com
testssl.sh example.com

# Check for sensitive data in responses
burpsuite # Analyze all responses for PII, credentials
```

**Prevention:**
- Use HTTPS everywhere
- Encrypt data at rest
- Use strong, up-to-date algorithms (AES-256, RSA-2048+)
- Proper key management
- Use bcrypt/Argon2 for passwords
- Disable caching for sensitive data

---

### A03:2021 - Injection 💉

**Description**: User-supplied data is not validated, filtered, or sanitized, allowing attackers to inject malicious code into queries or commands.

**Types:**
- SQL Injection
- NoSQL Injection
- Command Injection
- LDAP Injection
- XPath Injection
- XML Injection

**SQL Injection Example:**
```sql
-- Vulnerable code
SELECT * FROM users WHERE username = '$username' AND password = '$password'

-- Attack
username: admin' OR '1'='1' --
password: anything

-- Result
SELECT * FROM users WHERE username = 'admin' OR '1'='1' -- AND password = 'anything'
```

**Testing:**
```bash
# SQL injection with SQLMap
sqlmap -u "http://site.com/page.php?id=1" --dbs

# Command injection
; ls -la
| whoami
`cat /etc/passwd`

# NoSQL injection
{"username": {"$ne": null}, "password": {"$ne": null}}
```

**Prevention:**
- Use parameterized queries (prepared statements)
- Use ORM frameworks
- Validate and sanitize all inputs
- Use least privilege for database accounts
- Escape special characters

---

### A04:2021 - Insecure Design 🏗️

**Description**: Missing or ineffective security control design. Focus on risks related to design and architectural flaws.

**Examples:**
- No rate limiting on sensitive functions
- Missing account lockout
- Unencrypted password recovery tokens
- Lack of input validation by design
- Business logic flaws

**Common Flaws:**
```
- Race conditions
- Unlimited password reset attempts
- Price manipulation in shopping carts
- Bypassing multi-step processes
- Trust boundary violations
```

**Prevention:**
- Threat modeling
- Secure design patterns
- Security requirements in SDLC
- Security architecture review
- Paved road methodology

---

### A05:2021 - Security Misconfiguration ⚙️

**Description**: Missing security hardening, unnecessary features enabled, default accounts, verbose error messages.

**Common Issues:**
- Default credentials still enabled
- Directory listing enabled
- Verbose error messages
- Unnecessary features enabled
- Missing security headers
- Outdated software

**Testing:**
```bash
# Check security headers
curl -I https://example.com

# Common default credentials
admin:admin
admin:password
root:toor

# Check for directory listing
http://site.com/uploads/

# Information disclosure
http://site.com/phpinfo.php
http://site.com/.git/
http://site.com/.env
```

**Prevention:**
- Remove unnecessary features
- Change default credentials
- Implement security headers
- Keep software updated
- Disable directory listing
- Custom error pages

---

### A06:2021 - Vulnerable and Outdated Components 📦

**Description**: Using components with known vulnerabilities, unsupported or outdated software.

**Risk Areas:**
- Operating systems
- Web/application servers
- Database systems
- APIs
- Libraries and frameworks
- Plugins and extensions

**Testing:**
```bash
# Check for vulnerable dependencies
npm audit           # For Node.js
pip-audit          # For Python
bundler-audit      # For Ruby

# Version detection
whatweb site.com
wappalyzer         # Browser extension
retire.js          # JavaScript library checker
```

**Prevention:**
- Inventory all components and versions
- Monitor for vulnerabilities (CVE databases)
- Remove unused dependencies
- Regular patching and updates
- Use components from official sources
- Automated dependency checking

---

### A07:2021 - Identification and Authentication Failures 🔑

**Description**: Weak authentication mechanisms allowing attackers to compromise passwords, keys, or session tokens.

**Common Issues:**
- Weak password requirements
- No account lockout
- Credential stuffing
- Weak session management
- Missing MFA
- Exposing session IDs in URLs

**Testing:**
```bash
# Brute force attacks
hydra -l admin -P passwords.txt http-post-form

# Session management
# Check if session ID changes after login
# Check session timeout
# Check if session is in URL

# Password policy
# Test with weak passwords
# Check for account lockout
```

**Prevention:**
- Implement MFA
- Strong password policies
- Account lockout mechanisms
- No default credentials
- Secure session management
- Rate limiting on authentication

---

### A08:2021 - Software and Data Integrity Failures 🔏

**Description**: Code and infrastructure that doesn't protect against integrity violations, such as insecure deserialization.

**Examples:**
- Unsigned updates
- Insecure CI/CD pipelines
- Insecure deserialization
- Auto-update without integrity checks

**Common Attacks:**
```
- Supply chain attacks
- Malicious plugins/libraries
- Insecure deserialization exploits
- Man-in-the-middle on updates
```

**Prevention:**
- Digital signatures on updates
- Verify integrity of dependencies
- Secure CI/CD pipeline
- Don't deserialize untrusted data
- Implement integrity checks

---

### A09:2021 - Security Logging and Monitoring Failures 📊

**Description**: Insufficient logging, monitoring, and incident response allowing attacks to go undetected.

**Common Issues:**
- No logging of security events
- Logs not monitored
- Logs easily tampered with
- No alerting on suspicious activities
- Insufficient audit trail

**What to Log:**
```
✅ Authentication attempts (success/failure)
✅ Access control failures
✅ Input validation failures
✅ Application errors
✅ Security configuration changes
✅ Admin actions
```

**Prevention:**
- Log all authentication and authorization events
- Use centralized logging
- Implement monitoring and alerting
- Protect log integrity
- Regular log review
- Incident response procedures

---

### A10:2021 - Server-Side Request Forgery (SSRF) 🌐

**Description**: Web application fetches remote resources without validating user-supplied URLs, allowing attackers to force the application to send requests to unintended locations.

**Attack Scenarios:**
```bash
# Internal network scanning
http://site.com/fetch?url=http://192.168.1.1/

# Cloud metadata access
http://site.com/fetch?url=http://169.254.169.254/latest/meta-data/

# File reading
http://site.com/fetch?url=file:///etc/passwd

# Port scanning
http://site.com/fetch?url=http://internal-server:8080/
```

**Testing:**
```bash
# Test with various protocols
http://
https://
file://
gopher://
dict://

# Try localhost variations
localhost
127.0.0.1
127.0.0.2
0.0.0.0
```

**Prevention:**
- Whitelist allowed domains/protocols
- Validate and sanitize URLs
- Disable unnecessary URL schemas
- Network segmentation
- Implement firewall rules
- Use authentication for internal services

---

## 🎓 Learning Path

### Beginner (Week 1-4)
- [ ] Understand each OWASP Top 10 category
- [ ] Practice on [OWASP WebGoat](https://owasp.org/www-project-webgoat/)
- [ ] Complete [PortSwigger Academy](https://portswigger.net/web-security)
- [ ] Set up [DVWA](http://www.dvwa.co.uk/) for practice

### Intermediate (Week 5-8)
- [ ] Learn manual testing techniques
- [ ] Master [Burp Suite](../Web-Hacking-Tools/BurpSuite/)
- [ ] Practice on real applications (with permission)
- [ ] Complete web security CTF challenges

### Advanced (Week 9-12)
- [ ] Chain multiple vulnerabilities
- [ ] Bypass security controls
- [ ] Learn advanced exploitation techniques
- [ ] Start bug bounty hunting

---

## 🛠️ Essential Tools

### Web Proxies
- **Burp Suite** - Industry standard
- **OWASP ZAP** - Free and open source

### Vulnerability Scanners
- **Nikto** - Web server scanner
- **Nuclei** - Template-based scanner
- **w3af** - Web application attack framework

### Specialized Tools
- **SQLMap** - SQL injection
- **XSStrike** - XSS detection
- **Commix** - Command injection
- **NoSQLMap** - NoSQL injection

---

## 📚 Additional Resources

### Official Documentation
- [OWASP Top 10 Official](https://owasp.org/www-project-top-ten/)
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)

### Practice Platforms
- [PortSwigger Academy](https://portswigger.net/web-security)
- [OWASP WebGoat](https://owasp.org/www-project-webgoat/)
- [Damn Vulnerable Web Application (DVWA)](http://www.dvwa.co.uk/)
- [bWAPP](http://www.itsecgames.com/)

### Related Guides
- [Web Security](../Web-Security/) - Web application security fundamentals
- [Web Hacking Tools](../Web-Hacking-Tools/) - Tool tutorials
- [Bug Bounty](../BugBounty/) - Bug bounty hunting guide

---

## 📖 Detailed Guide

For comprehensive coverage of each vulnerability with examples, testing methodology, and remediation:

**[View Complete OWASP Top 10 Guide](./OWASP-Top-10/)**

---

<div class="notice--warning">
  <h4>⚖️ Legal Notice</h4>
  <p><strong>Only test applications you own or have explicit written permission to test.</strong> Unauthorized testing is illegal. Always practice on dedicated vulnerable applications and respect responsible disclosure practices.</p>
</div>

---

*Understanding OWASP Top 10 is the foundation of web application security. Master these, and you'll be well on your way to becoming a proficient security professional!* 🛡️
