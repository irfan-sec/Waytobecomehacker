# 💰 Bug Bounty Hunting Guide

> Complete guide to finding vulnerabilities and earning rewards ethically

---

## 📋 Overview

**Bug Bounty Programs** allow security researchers to report vulnerabilities to organizations and receive monetary rewards. It's a win-win: companies improve their security, and researchers get paid for their skills.

**Why Bug Bounty?**
- 💰 Earn money from hacking legally
- 🎓 Learn real-world security testing
- 🏆 Build reputation in security community
- 💼 Gain experience for security career
- 🤝 Help make the internet safer
- 🌐 Work from anywhere, flexible hours

---

## 🚀 Getting Started

### Prerequisites
```
Technical Skills:
- Web application security basics
- HTTP protocol understanding
- Common vulnerabilities (OWASP Top 10)
- Basic programming/scripting
- Networking fundamentals

Soft Skills:
- Attention to detail
- Persistence and patience
- Good documentation
- Clear communication
- Ethical mindset
```

### Before Your First Bug
```
1. Learn the Basics
   - Complete courses (PortSwigger Academy, TryHackMe)
   - Practice on legal platforms (HackTheBox, DVWA)
   - Read vulnerability disclosure reports
   
2. Choose Bug Bounty Platform
   - HackerOne
   - Bugcrowd
   - Intigriti
   - YesWeHack
   - Synack
   
3. Understand Legal Aspects
   - Read program rules carefully
   - Respect scope limitations
   - Follow responsible disclosure
   - Never access sensitive data unnecessarily
```

---

## 🎯 Choosing Programs

### Program Selection Criteria

**For Beginners:**
```
✅ Wide scope (large attack surface)
✅ Publicly disclosed programs
✅ Good documentation
✅ Responsive security team
✅ Clear vulnerability guidelines
✅ Educational content available

❌ Limited scope
❌ Unresponsive programs
❌ Unclear rules
❌ Very high competition
```

**Program Types:**
```
1. Public Programs
   - Anyone can participate
   - More competitive
   - Better for building reputation
   
2. Private Programs
   - Invitation only
   - Less competition
   - Often better rewards
   - Need good reputation to access

3. VDP (Vulnerability Disclosure Programs)
   - No monetary rewards
   - Good for beginners
   - Build reputation
   - Get experience
```

### Reading Program Scope

**Always Check:**
```
✅ In-Scope Domains
   - *.example.com
   - app.example.com
   - api.example.com

❌ Out-of-Scope Domains
   - third-party.example.com
   - legacy.example.com

✅ In-Scope Vulnerability Types
   - XSS, SQLi, CSRF
   - Authentication issues
   - Business logic flaws

❌ Out-of-Scope Vulnerabilities
   - Self-XSS
   - Clickjacking (often)
   - SPF/DMARC issues
   - Rate limiting (sometimes)

⚠️ Testing Restrictions
   - No DOS/DDOS
   - No social engineering
   - No physical access
   - Rate limiting requirements
```

---

## 🔍 Reconnaissance Phase

### 1. Subdomain Enumeration
```bash
# Passive enumeration
subfinder -d target.com -o subdomains.txt
amass enum -passive -d target.com
assetfinder target.com

# Active enumeration (if allowed)
sublist3r -d target.com
gobuster dns -d target.com -w wordlist.txt

# Certificate transparency
crt.sh
censys.io
shodan.io
```

### 2. Content Discovery
```bash
# Directory/file enumeration
ffuf -u https://target.com/FUZZ -w wordlist.txt
gobuster dir -u https://target.com -w wordlist.txt
dirsearch -u https://target.com

# Parameter discovery
arjun -u https://target.com/endpoint
paramspider -d target.com

# JavaScript analysis
linkfinder -i https://target.com/app.js
getJS --url https://target.com
```

### 3. Technology Stack Detection
```bash
# Identify technologies
whatweb https://target.com
wappalyzer https://target.com
builtwith.com

# Check for known CVEs
nuclei -u https://target.com -t cves/
nikto -h https://target.com
```

### 4. Google Dorking
```
# Find sensitive files
site:target.com ext:pdf
site:target.com ext:doc
site:target.com ext:xls
site:target.com ext:sql

# Find admin panels
site:target.com inurl:admin
site:target.com inurl:login
site:target.com inurl:dashboard

# Find exposed information
site:target.com "api_key"
site:target.com "password"
site:target.com "secret"
```

---

## 🐛 Common Vulnerabilities to Hunt

### 1. Cross-Site Scripting (XSS)

**Where to Look:**
```
- Input fields (search, comments, forms)
- URL parameters
- File upload features
- User profiles
- Error messages
- Reflected values in responses
```

**Quick Test Payloads:**
```javascript
// Basic tests
<script>alert(1)</script>
<img src=x onerror=alert(1)>
<svg onload=alert(1)>

// Filter bypass
<ScRiPt>alert(1)</ScRiPt>
<img src=x onerror="alert(1)">
<svg/onload=alert(1)>

// Advanced
<iframe src="javascript:alert(1)">
<object data="javascript:alert(1)">
```

**Impact to Report:**
```
High: Stored XSS
Medium: Reflected XSS
Low: Self-XSS (usually not accepted)
```

### 2. SQL Injection

**Where to Look:**
```
- Search functionality
- Login forms
- URL parameters (?id=1)
- Sorting/filtering
- Cookie values
- API endpoints
```

**Quick Test:**
```sql
-- Basic tests
' OR '1'='1
' OR 1=1--
admin' --
' UNION SELECT NULL--

-- Time-based detection
' AND SLEEP(5)--
'; WAITFOR DELAY '00:00:05'--

-- Error-based
' AND 1=CONVERT(int, @@version)--
```

**Testing Tools:**
```bash
sqlmap -u "https://target.com/page?id=1" --batch
sqlmap -r request.txt --batch
```

### 3. Insecure Direct Object Reference (IDOR)

**Where to Look:**
```
- User profiles (/user/123)
- Documents (/api/document/456)
- Orders (/order/789)
- Messages (/message/321)
- Settings (/settings?user_id=111)
```

**Testing Approach:**
```
1. Create two accounts
2. Note IDs/references in Account A
3. Try accessing with Account B
4. Test sequential IDs (1,2,3...)
5. Test UUIDs if present
6. Check API endpoints
```

**Example:**
```bash
# Normal request
GET /api/user/123/profile

# IDOR test
GET /api/user/124/profile  # Try other user's ID
GET /api/user/1/profile    # Try admin ID
```

### 4. Server-Side Request Forgery (SSRF)

**Where to Look:**
```
- URL parameters accepting URLs
- File upload from URL
- Webhook configurations
- PDF generators
- Import features
- Image processing
```

**Test Payloads:**
```bash
# Internal network
http://localhost:80
http://127.0.0.1:80
http://169.254.169.254/  # AWS metadata
http://192.168.1.1

# DNS rebinding
http://attacker-controlled.com

# Protocol smuggling
file:///etc/passwd
gopher://internal-host:6379
```

### 5. Authentication/Authorization Issues

**What to Test:**
```
- Password reset mechanism
- Account takeover vectors
- Session management
- OAuth implementation
- JWT token security
- 2FA bypass
- Privilege escalation
```

**Common Issues:**
```
- Predictable password reset tokens
- Password reset link doesn't expire
- Session fixation
- Weak session IDs
- Missing authorization checks
- Horizontal privilege escalation
```

### 6. Business Logic Flaws

**Examples:**
```
- Race conditions
- Negative prices
- Applying discounts multiple times
- Bypassing payment flows
- Referral abuse
- Bypassing rate limits
```

**Testing Approach:**
```
1. Understand the workflow
2. Identify assumptions
3. Think like a malicious user
4. Test edge cases
5. Try parallel requests (race conditions)
```

---

## 🛠️ Essential Tools

### Reconnaissance
```
- Subfinder - Subdomain enumeration
- Amass - Network mapping
- Assetfinder - Domain discovery
- httpx - HTTP toolkit
```

### Content Discovery
```
- ffuf - Fast web fuzzer
- Gobuster - Directory/file brute-forcer
- Arjun - Parameter discovery
- LinkFinder - Endpoint discovery
```

### Vulnerability Scanning
```
- Nuclei - Template-based scanner
- Nikto - Web server scanner
- Burp Suite - Comprehensive testing
- OWASP ZAP - Free alternative
```

### Exploitation
```
- SQLMap - SQL injection
- XSStrike - XSS detection
- Commix - Command injection
```

### Automation
```bash
# Create automation workflow
subfinder -d target.com | httpx | nuclei -t cves/

# Recon pipeline
echo target.com | subfinder | httpx -title -tech-detect | nuclei
```

---

## 📝 Writing Great Reports

### Report Structure

**1. Title**
```
✅ Good: "Stored XSS in Comment Section Allows Account Takeover"
❌ Bad: "XSS Found"
```

**2. Severity**
```
Critical - Remote code execution, full account takeover
High - SQL injection, stored XSS
Medium - Reflected XSS, IDOR
Low - Missing headers, information disclosure
Info - Configuration issues
```

**3. Summary**
```
Brief description of the vulnerability:
- What is the vulnerability?
- Where is it located?
- What is the impact?
```

**4. Steps to Reproduce**
```
Detailed, numbered steps:
1. Login to the application
2. Navigate to https://target.com/profile
3. In the "Bio" field, enter: <script>alert(1)</script>
4. Save the profile
5. View any user's profile page
6. Notice the XSS executes
```

**5. Proof of Concept**
```
- Screenshots/videos showing exploitation
- HTTP requests and responses
- Code snippets if applicable
- curl commands to reproduce
```

**6. Impact**
```
Explain the real-world impact:
- Can attacker steal user data?
- Can attacker take over accounts?
- What business impact does this have?
```

**7. Remediation**
```
Suggest how to fix:
- Use htmlspecialchars() to encode output
- Implement Content-Security-Policy
- Sanitize user input
```

### Report Example

````markdown
## Stored XSS in User Profile Bio

**Severity:** High

**Summary:**
The user profile "bio" field is vulnerable to stored cross-site scripting (XSS). An attacker can inject malicious JavaScript that executes when any user views the profile page.

**Steps to Reproduce:**
1. Log in to https://target.com
2. Navigate to Profile Settings
3. In the "Bio" field, enter:
   ```html
   <script>alert(document.cookie)</script>
   ```
4. Click "Save Profile"
5. Visit your profile page at https://target.com/profile/[your-id]
6. Observe the JavaScript execution

**Proof of Concept:**
[Attach screenshot showing alert box with cookies]

Request:
```http
POST /api/profile/update HTTP/1.1
Host: target.com
Cookie: session=abc123

{"bio": "<script>alert(document.cookie)</script>"}
```

**Impact:**
An attacker can:
- Steal session cookies of any user viewing the profile
- Perform actions on behalf of victims
- Deface the application
- Distribute malware

This could lead to full account takeover of any user who views the malicious profile.

**Remediation:**
1. Encode all user-supplied input before displaying:
   ```php
   echo htmlspecialchars($bio, ENT_QUOTES, 'UTF-8');
   ```
2. Implement Content-Security-Policy header
3. Use HTTPOnly flag on session cookies
````

---

## 💡 Tips for Success

### Finding More Bugs

**1. Focus on Lesser-Known Features**
```
- Admin panels
- API endpoints
- Mobile app endpoints
- Webhooks
- Forgotten subdomains
- Beta/staging environments (if in scope)
```

**2. Chain Vulnerabilities**
```
- Low severity bugs can become high severity when chained
- Example: CSRF + Self-XSS = Stored XSS
- Example: Open Redirect + OAuth = Account Takeover
```

**3. Think Outside the Box**
```
- Test business logic
- Look for race conditions
- Test file upload restrictions
- Check access controls thoroughly
- Test different user roles
```

**4. Automation with Caution**
```
- Automate reconnaissance
- Don't fully automate testing
- Manual testing finds unique bugs
- Understand what your tools do
```

### Increasing Acceptance Rate

**Do:**
```
✅ Read program policies completely
✅ Write clear, detailed reports
✅ Provide proof of concept
✅ Test thoroughly before submitting
✅ Explain business impact
✅ Be professional and respectful
✅ Accept duplicate closures gracefully
```

**Don't:**
```
❌ Test out-of-scope domains
❌ Submit low-quality reports
❌ Spam duplicate reports
❌ Be rude to security teams
❌ Report without reproducing
❌ Test what's explicitly excluded
```

---

## 📊 Bug Bounty Platforms

### Major Platforms

**HackerOne**
```
- Largest platform
- 1000+ programs
- Public and private programs
- Good support system
```

**Bugcrowd**
```
- Second largest
- Mix of public/private
- Crowdcontrol feature
- Good program variety
```

**Intigriti**
```
- European focus
- Quality over quantity
- Good payouts
- Community events
```

**Synack**
```
- Fully private
- Vetted researchers only
- Guaranteed payouts
- Consistent work
```

---

## 📚 Learning Resources

### Courses
```
- PortSwigger Web Security Academy (Free)
- PentesterLab (Paid)
- HackerOne Hacker101 (Free)
- Bugcrowd University (Free)
```

### Practice Platforms
```
- TryHackMe - Guided learning
- HackTheBox - Realistic machines
- PentesterLab - Web vulnerabilities
- PortSwigger Labs - Specific techniques
```

### Reading Material
```
- Real-World Bug Hunting by Peter Yaworski
- Bug Bounty Bootcamp by Vickie Li
- The Web Application Hacker's Handbook
```

### Communities
```
- r/bugbounty (Reddit)
- Bug Bounty Forum
- InfoSec Twitter
- Discord communities
```

---

## 🎯 30-Day Beginner Plan

### Week 1: Learning
```
- Complete PortSwigger Academy basics
- Learn about OWASP Top 10
- Set up testing environment
- Create accounts on bug bounty platforms
```

### Week 2: Practice
```
- Complete beginner labs (TryHackMe, PortSwigger)
- Learn to use Burp Suite effectively
- Practice on DVWA
- Read disclosed reports
```

### Week 3: Reconnaissance
```
- Choose 2-3 beginner-friendly programs
- Perform thorough reconnaissance
- Map attack surface
- Identify technologies
```

### Week 4: Hunting
```
- Start testing for common vulnerabilities
- Document findings
- Submit your first reports
- Learn from feedback
```

---

## ⚠️ Legal and Ethical Guidelines

**Always Remember:**
```
✅ Only test programs you're authorized to test
✅ Follow program rules and scope
✅ Report vulnerabilities responsibly
✅ Don't access/modify sensitive data
✅ Don't perform DoS attacks
✅ Respect rate limits
✅ Get explicit permission for social engineering

❌ Never test without permission
❌ Don't weaponize or share exploits publicly
❌ Don't blackmail or threaten companies
❌ Don't access other users' data
❌ Don't cause damage or disruption
```

---

*Hunt bugs ethically, report responsibly, get rewarded fairly. Make the internet safer, one bug at a time.*
