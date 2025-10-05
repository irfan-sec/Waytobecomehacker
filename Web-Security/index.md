---
title: "Web Application Security Fundamentals"
permalink: /Web-Security/
layout: single
author_profile: true
toc: true
toc_sticky: true
---

> Essential knowledge for securing and testing web applications

---

## 📋 Overview

Web application security is a critical aspect of cybersecurity. As more services move online, understanding how web applications work and their security implications is essential for both developers and security professionals.

**What You'll Learn:**
- 🏗️ Web application architecture
- 🔐 Common vulnerabilities and attacks
- 🛡️ Security best practices
- 🧪 Testing methodologies
- 🔧 Security tools and frameworks

---

## 🏗️ Web Application Architecture

### Client-Server Model
```
┌─────────────┐                    ┌─────────────┐
│   Browser   │ ←──── HTTP/S ────→ │ Web Server  │
│  (Client)   │                    │   (Apache,  │
│             │                    │    Nginx)   │
└─────────────┘                    └──────┬──────┘
                                          │
                                          ▼
                                   ┌─────────────┐
                                   │ Application │
                                   │   Server    │
                                   │ (PHP, Node, │
                                   │   Python)   │
                                   └──────┬──────┘
                                          │
                                          ▼
                                   ┌─────────────┐
                                   │  Database   │
                                   │   (MySQL,   │
                                   │  PostgreSQL)│
                                   └─────────────┘
```

### Key Components

**1. Frontend (Client-Side)**
- HTML - Structure
- CSS - Styling
- JavaScript - Interactivity

**2. Backend (Server-Side)**
- Application logic
- Database queries
- Authentication/Authorization
- Business logic

**3. Database Layer**
- Data storage
- User management
- Session storage

---

## 🌐 HTTP Protocol Fundamentals

### HTTP Request Structure
```http
GET /api/users/123 HTTP/1.1
Host: api.example.com
User-Agent: Mozilla/5.0
Accept: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
Cookie: session=abc123xyz
Connection: keep-alive
```

### HTTP Response Structure
```http
HTTP/1.1 200 OK
Date: Mon, 01 Jan 2024 12:00:00 GMT
Server: nginx/1.18.0
Content-Type: application/json
Content-Length: 256
Set-Cookie: session=abc123xyz; Secure; HttpOnly
X-Frame-Options: SAMEORIGIN

{"user": {"id": 123, "name": "John Doe"}}
```

### HTTP Methods
```bash
GET     # Retrieve data
POST    # Create data
PUT     # Update data (full replacement)
PATCH   # Partial update
DELETE  # Remove data
OPTIONS # Check available methods
HEAD    # Get headers only (no body)
```

### HTTP Status Codes
```
1xx - Informational
2xx - Success (200 OK, 201 Created)
3xx - Redirection (301 Moved, 302 Found)
4xx - Client Error (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found)
5xx - Server Error (500 Internal Server Error, 503 Service Unavailable)
```

---

## 🔐 Common Web Vulnerabilities

### 1. Cross-Site Scripting (XSS)
Injecting malicious scripts into web pages.

**Types:**
- **Reflected XSS** - Payload in URL/request
- **Stored XSS** - Payload saved in database
- **DOM-based XSS** - Client-side vulnerability

**Example:**
```html
<!-- Vulnerable code -->
<div>Welcome, <?php echo $_GET['name']; ?></div>

<!-- Attack -->
http://site.com/welcome?name=<script>alert('XSS')</script>

<!-- Prevention -->
<div>Welcome, <?php echo htmlspecialchars($_GET['name']); ?></div>
```

### 2. SQL Injection
Manipulating database queries through user input.

**Example:**
```sql
-- Vulnerable code
SELECT * FROM users WHERE username = '$username' AND password = '$password'

-- Attack
username: admin' OR '1'='1' --
password: anything

-- Prevention: Use prepared statements
$stmt = $pdo->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
$stmt->execute([$username, $password]);
```

### 3. Cross-Site Request Forgery (CSRF)
Forcing users to execute unwanted actions.

**Example:**
```html
<!-- Attacker's site -->
<img src="http://bank.com/transfer?to=attacker&amount=1000">

<!-- Prevention: Use CSRF tokens -->
<input type="hidden" name="csrf_token" value="random_token">
```

### 4. Insecure Direct Object References (IDOR)
Accessing unauthorized resources by manipulating parameters.

**Example:**
```bash
# Vulnerable
http://site.com/profile?user_id=123

# Attack: Change user_id to access others' profiles
http://site.com/profile?user_id=124

# Prevention: Check authorization
if ($current_user_id != $requested_user_id && !$is_admin) {
    return 403; // Forbidden
}
```

### 5. Server-Side Request Forgery (SSRF)
Making the server perform requests to unintended locations.

**Example:**
```bash
# Vulnerable
http://site.com/fetch?url=http://example.com

# Attack: Access internal resources
http://site.com/fetch?url=http://localhost/admin
http://site.com/fetch?url=http://169.254.169.254/latest/meta-data/

# Prevention: Whitelist allowed domains
```

---

## 🛡️ Security Best Practices

### Input Validation
```python
# Validate all user input
import re

def validate_email(email):
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None

def sanitize_input(user_input):
    # Remove dangerous characters
    return re.sub(r'[<>"\']', '', user_input)
```

### Output Encoding
```php
<?php
// HTML context
echo htmlspecialchars($user_input, ENT_QUOTES, 'UTF-8');

// JavaScript context
echo json_encode($user_input, JSON_HEX_TAG | JSON_HEX_AMP);

// URL context
echo urlencode($user_input);
?>
```

### Authentication & Session Management
```python
# Secure password hashing
import bcrypt

# Hash password
hashed = bcrypt.hashpw(password.encode(), bcrypt.gensalt())

# Verify password
if bcrypt.checkpw(password.encode(), hashed):
    # Password correct

# Secure session configuration
SESSION_COOKIE_SECURE = True      # HTTPS only
SESSION_COOKIE_HTTPONLY = True    # No JavaScript access
SESSION_COOKIE_SAMESITE = 'Strict' # CSRF protection
```

### Security Headers
```nginx
# Nginx configuration
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Content-Security-Policy "default-src 'self'" always;
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
```

---

## 🧪 Web Security Testing

### Reconnaissance
```bash
# Technology identification
whatweb target.com
wafw00f target.com

# Subdomain enumeration
sublist3r -d target.com
amass enum -d target.com

# Directory discovery
gobuster dir -u https://target.com -w wordlist.txt
ffuf -u https://target.com/FUZZ -w wordlist.txt
```

### Vulnerability Scanning
```bash
# Automated scanners
nikto -h https://target.com
nuclei -u https://target.com

# Web application scanner
burpsuite  # Use Spider and Scanner
zap.sh -quickurl https://target.com
```

### Manual Testing
```bash
# Using Burp Suite
1. Configure browser proxy (127.0.0.1:8080)
2. Browse the application
3. Analyze requests in Proxy > HTTP history
4. Send interesting requests to Repeater
5. Modify parameters and observe responses
```

---

## 🔧 Essential Tools

### Web Proxies
- **[Burp Suite](../Web-Hacking-Tools/BurpSuite/)** - Industry standard
- **[OWASP ZAP](../Web-Hacking-Tools/OWASP-ZAP/)** - Free alternative

### Scanners
- **[Nikto](../Web-Hacking-Tools/Nikto/)** - Web server scanner
- **[Nuclei](../Web-Hacking-Tools/Nuclei/)** - Template-based scanner

### Specialized Tools
- **[SQLMap](../Web-Hacking-Tools/SQLMap/)** - SQL injection
- **[XSStrike](../Web-Hacking-Tools/XSStrike/)** - XSS detection
- **[Gobuster](../Web-Hacking-Tools/Gobuster/)** - Directory brute forcing
- **[Hydra](../Web-Hacking-Tools/Hydra/)** - Password attacks

---

## 🎓 Learning Path

### Phase 1: Foundations (Weeks 1-4)
- [ ] Learn HTML, CSS, JavaScript basics
- [ ] Understand HTTP protocol
- [ ] Study client-server architecture
- [ ] Set up testing environment

### Phase 2: Common Vulnerabilities (Weeks 5-8)
- [ ] Master [OWASP Top 10](../OWASP/)
- [ ] Practice on [DVWA](http://www.dvwa.co.uk/)
- [ ] Complete [PortSwigger Academy](https://portswigger.net/web-security)
- [ ] Learn [Burp Suite](../Web-Hacking-Tools/BurpSuite/)

### Phase 3: Advanced Testing (Weeks 9-12)
- [ ] API security testing
- [ ] Authentication bypass techniques
- [ ] Business logic flaws
- [ ] Client-side attacks

### Phase 4: Real-World Practice (Months 4-6)
- [ ] Bug bounty programs
- [ ] HackTheBox web challenges
- [ ] TryHackMe web rooms
- [ ] Build your own vulnerable apps

---

## 📚 Learning Resources

### Online Courses
- **[PortSwigger Web Security Academy](https://portswigger.net/web-security)** - Free, comprehensive
- **[TryHackMe](https://tryhackme.com/)** - Hands-on labs
- **[HackTheBox](https://hackthebox.eu/)** - Real-world challenges

### Practice Platforms
- **[OWASP WebGoat](https://owasp.org/www-project-webgoat/)** - Educational app
- **[Damn Vulnerable Web Application (DVWA)](http://www.dvwa.co.uk/)** - Practice environment
- **[bWAPP](http://www.itsecgames.com/)** - Buggy web app
- **[Juice Shop](https://owasp.org/www-project-juice-shop/)** - Modern vulnerable app

### Books
- "The Web Application Hacker's Handbook" - Stuttard & Pinto
- "Web Security Testing Cookbook" - Paco Hope
- "Real-World Bug Hunting" - Peter Yaworski

### Documentation
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [MDN Web Docs](https://developer.mozilla.org/)

---

## 🔗 Related Topics

### Expand Your Knowledge
- **[OWASP Top 10](../OWASP/)** - Critical web vulnerabilities
- **[Web Hacking Tools](../Web-Hacking-Tools/)** - Essential tool guides
- **[Bug Bounty](../BugBounty/)** - Start earning rewards
- **[CTF Guide](../CTF/)** - Practice competitions
- **[Cryptography](../Cryptography/)** - Secure communications

---

## 📖 Detailed Guide

For comprehensive web security concepts and techniques:

**[View Complete Web Application Security Guide](./Web-Application-Security/)**

---

## 🎯 Testing Checklist

### Before Testing
- [ ] Get written authorization
- [ ] Understand scope and rules
- [ ] Set up testing environment
- [ ] Configure tools properly

### During Testing
- [ ] Document everything
- [ ] Test systematically
- [ ] Verify findings
- [ ] Note false positives

### After Testing
- [ ] Write clear reports
- [ ] Include proof of concepts
- [ ] Provide remediation advice
- [ ] Follow up on fixes

---

<div class="notice--warning">
  <h4>⚖️ Legal Notice</h4>
  <p><strong>ONLY test applications you own or have explicit written permission to test.</strong> Unauthorized security testing is illegal and can result in criminal prosecution. Always follow responsible disclosure practices and respect the scope of your engagement.</p>
</div>

---

*"Security is not a product, but a process."* - Bruce Schneier 🛡️
