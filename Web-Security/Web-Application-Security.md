# 🌐 Web Application Security Fundamentals

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
```html
<!-- HTML - Structure -->
<div id="login">
  <form action="/login" method="POST">
    <input type="text" name="username">
    <input type="password" name="password">
    <button type="submit">Login</button>
  </form>
</div>
```

```javascript
// JavaScript - Behavior
document.getElementById('login').addEventListener('submit', function(e) {
    e.preventDefault();
    // Client-side validation
    // Send AJAX request
});
```

```css
/* CSS - Presentation */
#login {
    max-width: 400px;
    margin: 50px auto;
}
```

**2. Backend (Server-Side)**
```python
# Example: Python Flask
from flask import Flask, request

@app.route('/login', methods=['POST'])
def login():
    username = request.form['username']
    password = request.form['password']
    # Authentication logic
    # Database queries
    return response
```

**3. Database Layer**
```sql
-- User authentication
SELECT * FROM users 
WHERE username = ? AND password = ?;

-- Data retrieval
SELECT id, name, email 
FROM customers 
WHERE status = 'active';
```

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
# GET - Retrieve data
GET /api/users

# POST - Create data
POST /api/users
Content-Type: application/json
{"name": "John", "email": "john@example.com"}

# PUT - Update data (full replacement)
PUT /api/users/123
{"name": "John Doe", "email": "john@example.com"}

# PATCH - Partial update
PATCH /api/users/123
{"email": "newemail@example.com"}

# DELETE - Remove data
DELETE /api/users/123

# OPTIONS - Check available methods
OPTIONS /api/users

# HEAD - Get headers only (no body)
HEAD /api/users
```

### HTTP Status Codes
```
1xx - Informational
100 Continue
101 Switching Protocols

2xx - Success
200 OK
201 Created
202 Accepted
204 No Content

3xx - Redirection
301 Moved Permanently
302 Found (Temporary)
304 Not Modified
307 Temporary Redirect

4xx - Client Errors
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
405 Method Not Allowed
429 Too Many Requests

5xx - Server Errors
500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
504 Gateway Timeout
```

---

## 🔐 Authentication and Authorization

### Authentication Methods

**1. Session-Based Authentication**
```python
# Login process
def login(username, password):
    if verify_credentials(username, password):
        session_id = generate_session_id()
        store_session(session_id, user_id)
        set_cookie('session_id', session_id)
        return True
    return False

# Authentication check
def is_authenticated(request):
    session_id = request.cookies.get('session_id')
    return validate_session(session_id)
```

**2. Token-Based Authentication (JWT)**
```javascript
// Generate JWT token
const jwt = require('jsonwebtoken');

function generateToken(userId) {
    const payload = {
        userId: userId,
        exp: Math.floor(Date.now() / 1000) + (60 * 60) // 1 hour
    };
    return jwt.sign(payload, SECRET_KEY);
}

// Verify JWT token
function verifyToken(token) {
    try {
        return jwt.verify(token, SECRET_KEY);
    } catch(err) {
        return null;
    }
}
```

**3. OAuth 2.0**
```
Flow:
1. User clicks "Login with Google"
2. Redirect to OAuth provider
3. User grants permission
4. Provider returns authorization code
5. Exchange code for access token
6. Use access token to access resources
```

### Authorization Patterns

**Role-Based Access Control (RBAC)**
```python
# Define roles and permissions
ROLES = {
    'admin': ['read', 'write', 'delete', 'manage_users'],
    'editor': ['read', 'write'],
    'viewer': ['read']
}

def check_permission(user_role, required_permission):
    return required_permission in ROLES.get(user_role, [])
```

**Attribute-Based Access Control (ABAC)**
```python
def can_access_resource(user, resource, action):
    # Check user attributes
    if user.department == resource.department:
        # Check action permission
        if action in user.permissions:
            return True
    return False
```

---

## 🛡️ Common Web Vulnerabilities

### 1. Cross-Site Scripting (XSS)

**Reflected XSS**
```html
<!-- Vulnerable code -->
<div>
    Search results for: <?php echo $_GET['search']; ?>
</div>

<!-- Attack -->
?search=<script>alert(document.cookie)</script>

<!-- Secure code -->
<div>
    Search results for: <?php echo htmlspecialchars($_GET['search']); ?>
</div>
```

**Stored XSS**
```javascript
// Vulnerable: Directly inserting user input
commentDiv.innerHTML = userComment;

// Secure: Using textContent or sanitization
commentDiv.textContent = userComment;
// Or use DOMPurify
commentDiv.innerHTML = DOMPurify.sanitize(userComment);
```

**DOM-based XSS**
```javascript
// Vulnerable
var hash = location.hash.substring(1);
document.write(hash);

// Secure
var hash = location.hash.substring(1);
document.getElementById('content').textContent = hash;
```

### 2. SQL Injection

**Vulnerable Code**
```python
# Bad - Direct string concatenation
query = "SELECT * FROM users WHERE username='" + username + "'"
cursor.execute(query)
```

**Secure Code**
```python
# Good - Parameterized query
query = "SELECT * FROM users WHERE username = ?"
cursor.execute(query, (username,))

# Even better - ORM
user = User.query.filter_by(username=username).first()
```

### 3. Cross-Site Request Forgery (CSRF)

**Attack Scenario**
```html
<!-- Attacker's malicious page -->
<img src="https://bank.com/transfer?to=attacker&amount=1000">
<!-- Executes if user is logged into bank.com -->
```

**Protection**
```html
<!-- CSRF Token in form -->
<form action="/transfer" method="POST">
    <input type="hidden" name="csrf_token" value="random_token_value">
    <input type="text" name="to">
    <input type="number" name="amount">
    <button type="submit">Transfer</button>
</form>
```

```python
# Server-side verification
def transfer(request):
    if request.form['csrf_token'] != session['csrf_token']:
        return "Invalid CSRF token", 403
    # Process transfer
```

### 4. Insecure Direct Object Reference (IDOR)

**Vulnerable**
```javascript
// GET /api/document/123
// User can access any document by changing ID
app.get('/api/document/:id', (req, res) => {
    const doc = getDocument(req.params.id);
    res.json(doc);
});
```

**Secure**
```javascript
// Verify ownership
app.get('/api/document/:id', authenticate, (req, res) => {
    const doc = getDocument(req.params.id);
    if (doc.owner_id !== req.user.id && !req.user.isAdmin) {
        return res.status(403).json({error: 'Forbidden'});
    }
    res.json(doc);
});
```

---

## 🔒 Security Headers

### Essential Security Headers
```nginx
# Nginx configuration

# Prevent clickjacking
add_header X-Frame-Options "SAMEORIGIN" always;

# XSS Protection
add_header X-XSS-Protection "1; mode=block" always;

# Prevent MIME sniffing
add_header X-Content-Type-Options "nosniff" always;

# Content Security Policy
add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline';" always;

# HTTPS enforcement
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;

# Referrer Policy
add_header Referrer-Policy "strict-origin-when-cross-origin" always;

# Permissions Policy
add_header Permissions-Policy "geolocation=(), microphone=(), camera=()" always;
```

### Content Security Policy (CSP)
```http
Content-Security-Policy: 
    default-src 'self';
    script-src 'self' https://trusted-cdn.com;
    style-src 'self' 'unsafe-inline';
    img-src 'self' data: https:;
    font-src 'self' https://fonts.gstatic.com;
    connect-src 'self' https://api.example.com;
    frame-ancestors 'none';
```

---

## 🧪 Security Testing Methodology

### 1. Reconnaissance
```bash
# Information gathering
whois target.com
nslookup target.com
dig target.com ANY

# Technology detection
whatweb https://target.com
wappalyzer https://target.com

# Subdomain enumeration
subfinder -d target.com
amass enum -d target.com
```

### 2. Mapping
```bash
# Directory enumeration
gobuster dir -u https://target.com -w wordlist.txt
ffuf -u https://target.com/FUZZ -w wordlist.txt

# Spider/crawl
gospider -s https://target.com -d 2
hakrawler -url https://target.com
```

### 3. Vulnerability Discovery
```bash
# Automated scanning
nuclei -u https://target.com -t cves/
nikto -h https://target.com

# Manual testing with Burp Suite
# Set up proxy
# Browse application
# Analyze requests/responses
```

### 4. Exploitation
```bash
# SQL injection
sqlmap -u "https://target.com/page?id=1"

# XSS testing
# Test payloads in all input fields
<script>alert(1)</script>
<img src=x onerror=alert(1)>

# Authentication testing
hydra -L users.txt -P pass.txt target.com http-post-form
```

---

## 🛠️ Security Tools

### Testing Tools
- **Burp Suite** - Comprehensive web testing
- **OWASP ZAP** - Free alternative to Burp
- **Postman** - API testing
- **SQLMap** - SQL injection
- **XSStrike** - XSS detection
- **Nuclei** - Vulnerability scanning

### Browser Extensions
- **Wappalyzer** - Technology detection
- **Cookie Editor** - Manage cookies
- **EditThisCookie** - Cookie manipulation
- **HackTools** - Pentesting tools in browser

### Defensive Tools
- **ModSecurity** - Web Application Firewall
- **Fail2Ban** - Intrusion prevention
- **OSSEC** - Host-based intrusion detection

---

## 📚 Best Practices

### Input Validation
```python
import re

def validate_email(email):
    pattern = r'^[\w\.-]+@[\w\.-]+\.\w+$'
    return re.match(pattern, email) is not None

def validate_input(user_input, input_type):
    if input_type == 'email':
        return validate_email(user_input)
    elif input_type == 'username':
        # Only alphanumeric and underscore
        return user_input.isalnum() or '_' in user_input
    # Add more validation as needed
```

### Output Encoding
```python
import html

# HTML encoding
safe_output = html.escape(user_input)

# URL encoding
from urllib.parse import quote
safe_url = quote(user_input)

# JavaScript encoding
import json
safe_js = json.dumps(user_input)
```

### Secure Password Storage
```python
import bcrypt

# Hash password
def hash_password(password):
    salt = bcrypt.gensalt()
    return bcrypt.hashpw(password.encode(), salt)

# Verify password
def verify_password(password, hashed):
    return bcrypt.checkpw(password.encode(), hashed)
```

---

## 📖 Learning Resources

- **PortSwigger Academy**: Free web security training
- **OWASP WebGoat**: Hands-on vulnerable application
- **TryHackMe**: Web hacking rooms
- **HackTheBox**: Web challenges
- **PentesterLab**: Web pentesting exercises
- **DVWA**: Damn Vulnerable Web Application

---

*Secure the web, one application at a time. Test ethically, code securely.*
