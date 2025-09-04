# 🔍 Nikto - Web Server Vulnerability Scanner

> Nikto is an open-source web server scanner that performs comprehensive tests against web servers for multiple items including dangerous files/programs, outdated server software and other problems.

---

## 📌 What is Nikto?

**Nikto** is a web server assessment tool that scans web servers for over 6700 potentially dangerous files/programs, checks for outdated versions of over 1250 servers, and looks for version-specific problems on over 270 servers.

### Key Features:
- 🎯 **Comprehensive Scanning** - Tests for dangerous files, outdated software, and configuration issues
- 📋 **Extensive Database** - Over 6700 vulnerability tests
- 🔧 **Plugin Architecture** - Modular design with customizable plugins
- 📊 **Multiple Output Formats** - HTML, XML, CSV, and more
- 🚀 **SSL Support** - HTTPS scanning capabilities
- 🔐 **Authentication Support** - Basic and NTLM authentication

---

## 🚀 Installation

### Kali Linux (Pre-installed)
```bash
# Nikto comes pre-installed on Kali Linux
nikto -h
```

### Ubuntu/Debian
```bash
sudo apt update
sudo apt install nikto
```

### From Source
```bash
# Clone from GitHub
git clone https://github.com/sullo/nikto.git
cd nikto/program
perl nikto.pl -h
```

### Using Docker
```bash
# Run Nikto in Docker
docker run --rm -it securecodebox/nikto -h http://target.com
```

---

## 🎯 Basic Usage

### Simple Web Server Scan
```bash
# Basic scan
nikto -h http://target.com

# HTTPS scan
nikto -h https://target.com

# Scan specific port
nikto -h http://target.com -p 8080

# Scan multiple ports
nikto -h http://target.com -p 80,443,8080,8443
```

### Output Options
```bash
# Save output to file
nikto -h http://target.com -o results.txt

# HTML output
nikto -h http://target.com -Format htm -o results.html

# XML output for parsing
nikto -h http://target.com -Format xml -o results.xml

# CSV output for spreadsheets
nikto -h http://target.com -Format csv -o results.csv
```

---

## 🔧 Advanced Configuration

### Authentication
```bash
# HTTP Basic authentication
nikto -h http://target.com -id username:password

# NTLM authentication
nikto -h http://target.com -id domain\username:password

# Custom authentication header
nikto -h http://target.com -H "Authorization: Bearer token123"
```

### Proxy Configuration
```bash
# HTTP proxy
nikto -h http://target.com -useproxy http://proxy:8080

# SOCKS proxy
nikto -h http://target.com -useproxy socks://proxy:1080

# Proxy with authentication
nikto -h http://target.com -useproxy http://user:pass@proxy:8080
```

### SSL/TLS Options
```bash
# Ignore SSL certificate errors
nikto -h https://target.com -ssl

# Force SSL/TLS version
nikto -h https://target.com -ssl -vhost target.com

# Test SSL certificate
nikto -h https://target.com -Plugins sslinfo
```

---

## 🎭 Customization and Plugins

### Plugin Management
```bash
# List available plugins
nikto -list-plugins

# Use specific plugins only
nikto -h http://target.com -Plugins "headers,outdated"

# Exclude specific plugins
nikto -h http://target.com -Plugins "@@ALL,-dos"

# Show plugin information
nikto -h http://target.com -Plugins "headers" -Display V
```

### Common Useful Plugins
```bash
# Headers analysis
nikto -h http://target.com -Plugins headers

# Check for outdated software
nikto -h http://target.com -Plugins outdated

# SSL/TLS information
nikto -h https://target.com -Plugins sslinfo

# Directory indexing
nikto -h http://target.com -Plugins dir_indexing

# Server information disclosure
nikto -h http://target.com -Plugins info

# Test for Apache modules
nikto -h http://target.com -Plugins apache_expect_xss
```

### Vulnerability Testing
```bash
# Test for specific vulnerabilities
nikto -h http://target.com -Plugins "shellshock,heartbleed"

# Directory traversal tests
nikto -h http://target.com -Plugins dir_traversal

# Cross-site scripting tests
nikto -h http://target.com -Plugins xss

# SQL injection basic tests
nikto -h http://target.com -Plugins sql_injection
```

---

## 💡 Advanced Techniques

### Virtual Host Testing
```bash
# Test multiple virtual hosts
nikto -h http://target.com -vhost www.target.com,app.target.com,admin.target.com

# Force specific Host header
nikto -h http://192.168.1.100 -vhost target.com
```

### Custom User Agents and Headers
```bash
# Custom User-Agent
nikto -h http://target.com -useragent "Custom-Scanner/1.0"

# Multiple custom headers
nikto -h http://target.com -H "X-Forwarded-For: 127.0.0.1" -H "X-Real-IP: 127.0.0.1"

# Simulate mobile browser
nikto -h http://target.com -useragent "Mozilla/5.0 (iPhone; CPU iPhone OS 14_0 like Mac OS X)"
```

### Performance Tuning
```bash
# Adjust timeout (default: 10 seconds)
nikto -h http://target.com -timeout 30

# Control request delay
nikto -h http://target.com -Pause 2

# Limit maximum number of redirects
nikto -h http://target.com -maxtime 3600
```

### Evasion Techniques
```bash
# Random User-Agent rotation
nikto -h http://target.com -useragent "@@RANDOM"

# Encode URLs
nikto -h http://target.com -evasion 1

# Use different evasion techniques
# 1: Random URI encoding
# 2: Directory self-reference
# 3: Premature URL ending
# 4: Prepend long random string
# 5: Fake parameter
# 6: TAB as request spacer
# 7: Change case
# 8: Use Windows directory separator

nikto -h http://target.com -evasion 1,2,3,4
```

---

## 📊 Understanding Results

### Vulnerability Severity
Nikto results include various finding types:

**🔴 High Risk:**
- Server vulnerabilities with known exploits
- Dangerous file exposures
- Default credentials

**🟡 Medium Risk:**
- Information disclosure
- Outdated software versions
- Insecure configurations

**🔵 Low Risk/Info:**
- Server information
- Directory listings
- Missing security headers

### Common Findings
```bash
# Server information disclosure
"Server: Apache/2.4.7 (Ubuntu)"

# Outdated software
"Apache/2.4.7 appears to be outdated (current is at least Apache/2.4.41)"

# Dangerous files
"/admin/: Directory indexing found."

# Missing security headers
"Missing X-Frame-Options header"

# Default files
"/icons/: Directory indexing found."
```

---

## 🎯 Real-World Scenarios

### 1. Initial Web Application Assessment
```bash
# Comprehensive initial scan
nikto -h http://target.com -Format htm -o nikto_scan.html

# Follow up with SSL analysis if HTTPS
nikto -h https://target.com -Plugins sslinfo -Format txt -o ssl_analysis.txt

# Test for common vulnerabilities
nikto -h http://target.com -Plugins "shellshock,heartbleed,expect" -o vuln_check.txt
```

### 2. Virtual Host Discovery and Testing
```bash
# First, discover virtual hosts with other tools
# gobuster vhost -u http://target.com -w wordlist.txt

# Then test discovered hosts
nikto -h http://target.com -vhost admin.target.com,api.target.com,dev.target.com
```

### 3. CI/CD Integration
```bash
# Automated security scanning in pipeline
nikto -h http://staging.myapp.com -Format xml -o nikto_results.xml

# Parse results for critical findings
grep -i "critical\|high" nikto_results.xml

# Fail build if critical vulnerabilities found
if grep -qi "critical" nikto_results.xml; then exit 1; fi
```

### 4. Compliance Scanning
```bash
# Security header compliance
nikto -h https://target.com -Plugins headers -Format csv -o headers_check.csv

# Check for outdated software
nikto -h http://target.com -Plugins outdated -Format txt -o outdated_software.txt
```

---

## 🛠️ Integration with Other Tools

### With Nmap
```bash
# First, discover web servers with Nmap
nmap -sS -p 80,443,8080,8443 target.com

# Then scan discovered services with Nikto
nmap -p 80,443,8080,8443 target.com --script http-enum
nikto -h http://target.com -p 80,8080
nikto -h https://target.com -p 443,8443
```

### With Burp Suite
```bash
# Use Nikto through Burp proxy for manual review
nikto -h http://target.com -useproxy http://127.0.0.1:8080

# This allows you to:
# - Review all requests in Burp
# - Manually test interesting findings
# - Add requests to Burp scope for further testing
```

### With Metasploit
```bash
# Use Nikto results to guide Metasploit testing
nikto -h http://target.com -Format xml -o nikto.xml

# Import findings and look for exploitable vulnerabilities
# Then use appropriate Metasploit modules
```

---

## 📝 Custom Configuration

### Configuration File
Edit `/etc/nikto.conf` or create custom config:

```bash
# Example custom configuration
CLIOPTS=-Display 1234EP -o nikto_output.txt -Format htm
NIKTODB=/usr/share/nikto/databases/
PLUGINDIR=/usr/share/nikto/plugins/

# Custom User-Agents
@@USERAGENTS=Mozilla/5.0 (compatible; CustomBot/1.0)
@@USERAGENTS=Mozilla/5.0 (X11; Linux x86_64) Custom Scanner

# Custom headers
@@HEADERS=X-Scanner: Nikto
@@HEADERS=X-Test: Security-Assessment
```

### Custom Database Updates
```bash
# Update Nikto databases
nikto -update

# Manual database location
nikto -h http://target.com -config /path/to/custom/nikto.conf

# Use specific database version
nikto -h http://target.com -dbcheck
```

---

## 🚀 Automation and Scripting

### Batch Scanning
```bash
# Create target list
echo -e "http://target1.com\nhttp://target2.com\nhttp://target3.com" > targets.txt

# Scan multiple targets
while read target; do
    echo "Scanning $target"
    nikto -h "$target" -o "nikto_$(echo $target | sed 's/[^a-zA-Z0-9]/_/g').txt"
done < targets.txt
```

### Automated Reporting
```bash
#!/bin/bash
# automated_nikto_scan.sh

TARGET=$1
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
OUTPUT_DIR="nikto_results_$TIMESTAMP"

mkdir -p "$OUTPUT_DIR"

echo "Starting Nikto scan of $TARGET"

# Basic scan
nikto -h "$TARGET" -Format htm -o "$OUTPUT_DIR/basic_scan.html"

# SSL analysis if HTTPS
if [[ $TARGET == https* ]]; then
    nikto -h "$TARGET" -Plugins sslinfo -Format txt -o "$OUTPUT_DIR/ssl_analysis.txt"
fi

# Security headers check
nikto -h "$TARGET" -Plugins headers -Format csv -o "$OUTPUT_DIR/headers.csv"

# Vulnerability check
nikto -h "$TARGET" -Plugins "shellshock,heartbleed" -Format txt -o "$OUTPUT_DIR/vulnerabilities.txt"

echo "Scan complete. Results in $OUTPUT_DIR/"
```

---

## 🎓 Learning Path

### Beginner Level (Week 1-2):
- [ ] Install Nikto and run basic scans
- [ ] Understand different output formats
- [ ] Learn to interpret common findings
- [ ] Practice on intentionally vulnerable applications (DVWA, WebGoat)

### Intermediate Level (Week 3-4):
- [ ] Master plugin system and customization
- [ ] Learn authentication and proxy configuration
- [ ] Practice virtual host testing
- [ ] Integrate with other scanning tools

### Advanced Level (Week 5-8):
- [ ] Create custom plugins and configurations
- [ ] Implement automated scanning workflows
- [ ] Master evasion techniques
- [ ] Integrate into CI/CD pipelines

---

## 📚 Recommended Resources

### Practice Environments:
- **[DVWA](http://www.dvwa.co.uk/)** - Damn Vulnerable Web Application
- **[WebGoat](https://owasp.org/www-project-webgoat/)** - OWASP learning environment
- **[Metasploitable](https://sourceforge.net/projects/metasploitable/)** - Vulnerable Linux distribution
- **[VulnHub](https://www.vulnhub.com/)** - Vulnerable VMs for practice

### Documentation:
- **[Official Nikto Documentation](https://github.com/sullo/nikto/wiki)**
- **[Nikto Plugin Documentation](https://github.com/sullo/nikto/tree/master/program/plugins)**
- **[OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)**

---

## 🛡️ Defensive Measures

Understanding Nikto helps in defense:

### Detection Signatures
```bash
# Common Nikto signatures in logs:
# User-Agent: Mozilla/5.0 (compatible; Nikto/2.x.x)
# Requests for common vulnerability paths:
# - /admin/
# - /backup/
# - /test/
# - /.git/
# - /robots.txt
# - /phpinfo.php
```

### Mitigation Strategies
```bash
# Rate limiting
# Web Application Firewall (WAF) rules
# Remove unnecessary files and directories
# Update software regularly
# Implement proper security headers
# Configure proper error pages
```

---

## 🔗 Quick Reference Commands

```bash
# Basic scan
nikto -h http://target.com

# HTTPS scan with SSL info
nikto -h https://target.com -Plugins sslinfo

# Authenticated scan
nikto -h http://target.com -id username:password

# Proxy scan
nikto -h http://target.com -useproxy http://127.0.0.1:8080

# Multiple hosts
nikto -h http://target.com -vhost www.target.com,admin.target.com

# Specific plugins only
nikto -h http://target.com -Plugins "headers,outdated,dir_indexing"

# HTML output
nikto -h http://target.com -Format htm -o results.html

# Stealth scan with evasion
nikto -h http://target.com -evasion 1,2,7 -useragent "@@RANDOM"

# Update databases
nikto -update

# List all plugins
nikto -list-plugins
```

---

## ⚠️ Best Practices and Limitations

### ✅ Best Practices:
- Always scan with appropriate authorization
- Start with basic scans before using aggressive plugins
- Review and validate all findings manually
- Keep Nikto databases updated
- Use appropriate output formats for reporting

### ⚠️ Limitations:
- May generate false positives
- Cannot detect all vulnerability types
- Limited to surface-level scanning
- May miss custom or complex vulnerabilities
- Can be easily detected by security systems

### 🚫 Avoid These Mistakes:
- Don't rely solely on automated scanning
- Don't ignore false positive analysis
- Don't run aggressive scans on production without permission
- Don't assume all findings are actual vulnerabilities

---

<div class="notice--info">
  <h4>💡 Pro Tip</h4>
  <p>Nikto is most effective as part of a comprehensive security assessment. Use it for initial reconnaissance and vulnerability discovery, then follow up with manual testing and specialized tools for deeper analysis.</p>
</div>

---

*Nikto is a valuable tool for web server security assessment. Use it responsibly to identify and help remediate security issues in web applications and servers.*