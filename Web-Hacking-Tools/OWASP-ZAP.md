# 🛡️ OWASP ZAP - Complete Web Security Scanner Guide

> OWASP Zed Attack Proxy (ZAP) is a free, open-source penetration testing tool for finding vulnerabilities in web applications.

---

## 📌 What is OWASP ZAP?

**OWASP ZAP** (Zed Attack Proxy) is one of the world's most popular free security tools and is actively maintained by hundreds of international volunteers. It can help you automatically find security vulnerabilities in your web applications while you are developing and testing your applications.

### Key Features:
- ✅ **Intercepting Proxy** - See and modify all traffic between browser and application
- ✅ **Active & Passive Scanning** - Automated vulnerability detection
- ✅ **Spider/Crawler** - Discovers all pages and functionality
- ✅ **Fuzzer** - Tests inputs for unexpected responses
- ✅ **REST API** - Automate security testing in CI/CD pipelines

---

## 🚀 Installation & Setup

### Method 1: Kali Linux (Pre-installed)
```bash
# ZAP is pre-installed on Kali Linux
zaproxy
# or
owasp-zap
```

### Method 2: Download from Official Site
1. Visit [https://www.zaproxy.org/download/](https://www.zaproxy.org/download/)
2. Download for your OS (Windows, macOS, Linux)
3. Install following the platform-specific instructions

### Method 3: Docker Installation
```bash
# Pull the latest ZAP Docker image
docker pull owasp/zap2docker-stable

# Run ZAP in headless mode
docker run -t owasp/zap2docker-stable zap-baseline.py -t http://target-site.com
```

---

## 🎯 Getting Started - Your First Scan

### 1. Launch ZAP
```bash
# Start ZAP with GUI
zaproxy

# Start ZAP headless (command line only)
zap.sh -cmd -port 8080
```

### 2. Basic Automated Scan
1. **Launch ZAP** and click "Start"
2. **Enter target URL** in the "Quick Start" tab
3. **Choose attack mode**:
   - **Safe Mode**: Only passive scanning
   - **Protected Mode**: Limited to defined scope
   - **Standard Mode**: Full features, manual control
   - **Attack Mode**: Aggressive testing (use carefully!)
4. **Click "Attack"** to start automated scanning

---

## 🔧 Core Features & Workflow

### 1. Proxy Configuration
ZAP acts as a proxy between your browser and the target application:

```bash
# Default ZAP proxy settings
Proxy: 127.0.0.1:8080
```

**Browser Setup:**
1. Configure browser proxy settings
2. Navigate to `http://zap/` to download ZAP certificate
3. Install certificate in browser for HTTPS interception

### 2. Spidering (Crawling)
Discover all application pages and functionality:

```bash
# Command line spider
zap-cli spider http://target-site.com

# Or use GUI: Tools > Spider
```

### 3. Active Scanning
Actively test for vulnerabilities:

```bash
# Command line active scan
zap-cli active-scan http://target-site.com

# Monitor scan progress
zap-cli status
```

### 4. Passive Scanning
Analyze requests/responses for issues without sending additional requests:
- Runs automatically as you browse
- Identifies issues like missing security headers
- No risk of damaging target application

---

## 💡 Advanced Techniques

### 1. Authentication Testing
Test applications requiring login:

**Form-based Authentication:**
1. Navigate to **Context** > **Add Context**
2. Set **Include in Context** patterns
3. Configure **Authentication** method
4. Set up **Session Management**
5. Create **Users** for testing

### 2. Custom Fuzzing
Test specific parameters with custom payloads:

```bash
# Access Fuzzer through right-click on request
# Set payload positions with $ markers
# Choose from built-in wordlists or custom
```

### 3. API Testing
Test REST APIs and web services:

1. **Import API definition** (OpenAPI/Swagger)
2. **Configure authentication** (API keys, tokens)
3. **Run automated scans** on endpoints
4. **Review results** for API-specific vulnerabilities

### 4. Scripting & Automation
Automate ZAP with scripts:

```javascript
// Example ZAP script (JavaScript)
function scan(msg) {
    // Custom scanning logic
    var uri = msg.getRequestHeader().getURI().toString();
    if (uri.contains("admin")) {
        // Flag potential admin interface
        return;
    }
}
```

---

## 📊 Understanding Results

### Vulnerability Severity Levels:
- 🔴 **High**: Critical security issues requiring immediate attention
- 🟡 **Medium**: Significant security concerns
- 🔵 **Low**: Minor security issues or best practice violations
- ℹ️ **Informational**: No direct security impact

### Common Vulnerability Types ZAP Detects:
- **SQL Injection** - Database query manipulation
- **Cross-Site Scripting (XSS)** - Malicious script injection
- **Cross-Site Request Forgery (CSRF)** - Unauthorized command transmission
- **Path Traversal** - Unauthorized file access
- **Security Headers Missing** - Missing protective HTTP headers

---

## 🛠️ Integration with CI/CD

### Baseline Scan in Pipeline
```bash
# Jenkins/GitLab CI example
docker run -t owasp/zap2docker-stable zap-baseline.py \
    -t https://your-app.com \
    -g gen.conf \
    -r baseline-report.html
```

### Full Scan Automation
```python
#!/usr/bin/env python3
from zapv2 import ZAPv2

# Connect to ZAP
zap = ZAPv2(proxies={'http': 'http://127.0.0.1:8080'})

# Spider the target
target = 'https://your-app.com'
zap.spider.scan(target)

# Wait for spider to complete
while int(zap.spider.status()) < 100:
    print(f'Spider progress: {zap.spider.status()}%')
    time.sleep(2)

# Start active scan
zap.ascan.scan(target)

# Generate report
with open('zap-report.html', 'w') as f:
    f.write(zap.core.htmlreport())
```

---

## 🎓 Learning Path

### Beginner Level (Week 1-2):
- [ ] Install ZAP and configure browser proxy
- [ ] Practice on DVWA or WebGoat
- [ ] Learn to interpret basic scan results
- [ ] Understand difference between active/passive scanning

### Intermediate Level (Week 3-6):
- [ ] Configure authentication for protected apps
- [ ] Learn manual testing with intercepting proxy
- [ ] Practice fuzzing techniques
- [ ] Set up automated baseline scans

### Advanced Level (Week 7-12):
- [ ] Write custom ZAP scripts
- [ ] Integrate ZAP into CI/CD pipelines
- [ ] Perform API security testing
- [ ] Contribute to ZAP community

---

## 📚 Recommended Resources

### Official Documentation:
- **[ZAP User Guide](https://www.zaproxy.org/docs/)** - Comprehensive documentation
- **[ZAP in Ten](https://www.zaproxy.org/zap-in-ten/)** - Quick video tutorials
- **[ZAP Blog](https://www.zaproxy.org/blog/)** - Latest features and tips

### Practice Environments:
- **[DVWA](http://www.dvwa.co.uk/)** - Damn Vulnerable Web Application
- **[WebGoat](https://owasp.org/www-project-webgoat/)** - OWASP learning environment
- **[Mutillidae](https://sourceforge.net/projects/mutillidae/)** - Vulnerable web app

### Video Training:
- **[OWASP ZAP Tutorials](https://www.youtube.com/c/psiinon)** - Official YouTube channel
- **[TryHackMe ZAP Rooms](https://tryhackme.com/)** - Hands-on labs

---

## 🔍 Common Use Cases

### 1. Development Testing
```bash
# Quick scan during development
zap-baseline.py -t http://localhost:3000 -g gen.conf
```

### 2. Penetration Testing
- Comprehensive vulnerability assessment
- Manual testing with intercepting proxy
- Custom payload fuzzing
- Authentication bypass testing

### 3. Bug Bounty Hunting
- Initial reconnaissance and scanning
- Finding low-hanging fruit vulnerabilities
- Validating potential security issues
- Generating professional reports

### 4. Compliance Testing
- OWASP Top 10 vulnerability assessment
- Security header compliance checking
- SSL/TLS configuration testing
- Input validation testing

---

## ⚠️ Best Practices & Warnings

### ✅ DO:
- Always get written permission before testing
- Start with passive scanning on production systems
- Use safe mode when learning
- Keep ZAP updated to latest version
- Review all findings manually

### ❌ DON'T:
- Run active scans on production without permission
- Use attack mode on systems you don't own
- Ignore rate limiting and respectful testing
- Trust automated results without manual verification

---

## 🐛 Troubleshooting

### Common Issues:
1. **Certificate Warnings**: Install ZAP certificate in browser
2. **Slow Scanning**: Adjust scan policy and thread count
3. **False Positives**: Review findings manually
4. **Memory Issues**: Increase JVM heap size

```bash
# Increase memory allocation
export JAVA_OPTS="-Xmx4g"
zaproxy
```

---

<div class="notice--info">
  <h4>💡 Pro Tip</h4>
  <p>ZAP is most effective when combined with manual testing. Use automated scans to find obvious issues, then dive deeper with manual techniques using the intercepting proxy and fuzzer features.</p>
</div>

---

*OWASP ZAP is maintained by volunteers and is completely free. Consider contributing to the project or donating to support continued development.*