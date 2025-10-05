# ⚡ Nuclei - Fast Vulnerability Scanner

> Community-powered vulnerability scanner with 1000+ templates

---

## 📋 Overview

**Nuclei** is a fast and customizable vulnerability scanner based on simple YAML templates. It's developed by ProjectDiscovery and enables security researchers to create custom templates for vulnerabilities they discover. With over 1000+ community-contributed templates, Nuclei can detect a wide range of security issues across web applications, networks, and infrastructure.

**Key Features:**
- 🚀 Extremely fast scanning (Go-based)
- 📝 1000+ ready-to-use templates
- 🎨 Simple YAML-based template syntax
- 🔄 Automatic template updates
- 🌐 Multiple protocol support (HTTP, DNS, TCP, etc.)
- 📊 Comprehensive reporting options
- 🎯 Low false positive rate

---

## 🛠️ Installation

### Using Go
```bash
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
```

### On Kali Linux
```bash
sudo apt update
sudo apt install nuclei -y
```

### Using Binary
```bash
# Download latest release
wget https://github.com/projectdiscovery/nuclei/releases/download/v3.0.0/nuclei_3.0.0_linux_amd64.zip

# Extract and install
unzip nuclei_3.0.0_linux_amd64.zip
sudo mv nuclei /usr/local/bin/
```

### Docker
```bash
docker pull projectdiscovery/nuclei:latest

# Run Nuclei in Docker
docker run -it projectdiscovery/nuclei:latest -u https://example.com
```

### Verify Installation
```bash
nuclei -version
```

---

## 🚀 Quick Start

### Update Templates
```bash
# Update to latest templates
nuclei -update-templates

# Check installed templates
nuclei -tl
```

### Basic Scanning
```bash
# Scan single target
nuclei -u https://example.com

# Scan multiple targets from file
nuclei -l targets.txt

# Scan with specific template
nuclei -u https://example.com -t cves/2021/CVE-2021-44228.yaml

# Scan specific directory of templates
nuclei -u https://example.com -t vulnerabilities/
```

---

## 📚 Template Categories

### CVE Templates
```bash
# Scan for known CVEs
nuclei -u https://example.com -t cves/

# Specific CVE
nuclei -u https://example.com -t cves/2021/

# Critical CVEs only
nuclei -u https://example.com -t cves/ -severity critical
```

### Vulnerability Templates
```bash
# Generic vulnerabilities
nuclei -u https://example.com -t vulnerabilities/

# SQL injection
nuclei -u https://example.com -t vulnerabilities/sqli/

# XSS
nuclei -u https://example.com -t vulnerabilities/xss/

# LFI/RFI
nuclei -u https://example.com -t vulnerabilities/lfi/
```

### Misconfigurations
```bash
# Check for misconfigurations
nuclei -u https://example.com -t misconfiguration/

# Exposed panels
nuclei -u https://example.com -t exposures/

# Default credentials
nuclei -u https://example.com -t default-logins/
```

### Technology Detection
```bash
# Detect technologies
nuclei -u https://example.com -t technologies/

# Identify CMS
nuclei -u https://example.com -t technologies/cms/

# Web servers
nuclei -u https://example.com -t technologies/webserver/
```

---

## 🎯 Advanced Usage

### Severity Filtering
```bash
# Critical only
nuclei -u https://example.com -severity critical

# High and critical
nuclei -u https://example.com -severity critical,high

# Exclude info
nuclei -u https://example.com -severity critical,high,medium,low
```

### Tags and Filtering
```bash
# Scan by tags
nuclei -u https://example.com -tags cve,oast

# Exclude specific tags
nuclei -u https://example.com -etags dos,fuzz

# Author filtering
nuclei -u https://example.com -author geeknik,pikpikcu

# Exclude templates
nuclei -u https://example.com -exclude-templates cves/2021/CVE-2021-1234.yaml
```

### Output and Reporting
```bash
# JSON output
nuclei -u https://example.com -json -o results.json

# Markdown report
nuclei -u https://example.com -markdown -o report.md

# SARIF format
nuclei -u https://example.com -sarif -o nuclei.sarif

# Multiple outputs
nuclei -u https://example.com -json -markdown -o results
```

### Rate Limiting and Performance
```bash
# Increase concurrency (default 25)
nuclei -u https://example.com -c 50

# Rate limit (requests per second)
nuclei -u https://example.com -rate-limit 10

# Timeout per request
nuclei -u https://example.com -timeout 10

# Retries on failure
nuclei -u https://example.com -retries 3
```

---

## 💡 Real-World Scenarios

### Scenario 1: Quick Vulnerability Scan
```bash
# Comprehensive scan for common issues
nuclei -u https://target.com \
    -t cves/ -t vulnerabilities/ -t misconfiguration/ \
    -severity critical,high \
    -json -o scan-results.json
```

### Scenario 2: Log4Shell Detection
```bash
# Scan for Log4j vulnerability
nuclei -l targets.txt \
    -t cves/2021/CVE-2021-44228.yaml \
    -t cves/2021/CVE-2021-45046.yaml \
    -json -o log4shell-scan.json
```

### Scenario 3: Infrastructure Audit
```bash
# Check for exposed services and panels
nuclei -l infrastructure.txt \
    -t exposures/ \
    -t misconfiguration/ \
    -t default-logins/ \
    -severity high,critical \
    -markdown -o audit-report.md
```

### Scenario 4: Bug Bounty Recon
```bash
# Comprehensive bug bounty scan
cat subdomains.txt | \
nuclei -t cves/ \
    -t vulnerabilities/ \
    -t exposures/ \
    -severity critical,high,medium \
    -c 50 \
    -json -o bounty-results.json
```

### Scenario 5: CI/CD Integration
```bash
# Automated security scanning in pipeline
nuclei -l deployment-urls.txt \
    -t cves/ -t vulnerabilities/ \
    -severity critical,high \
    -json -o pipeline-scan.json

# Exit with error if critical found
nuclei -u https://staging.example.com \
    -severity critical \
    -exit-on-first-critical
```

---

## 📝 Creating Custom Templates

### Basic Template Structure
```yaml
id: custom-vulnerability

info:
  name: Custom Vulnerability Check
  author: your-name
  severity: high
  description: Description of the vulnerability
  tags: custom,web

requests:
  - method: GET
    path:
      - "{{BaseURL}}/vulnerable-endpoint"
    
    matchers:
      - type: status
        status:
          - 200
      
      - type: word
        words:
          - "vulnerable pattern"
        condition: and
```

### Advanced Template Example
```yaml
id: advanced-template

info:
  name: Advanced Detection
  author: security-researcher
  severity: critical
  reference:
    - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-XXXX-XXXX
  tags: cve,rce

requests:
  - raw:
      - |
        POST /api/vulnerable HTTP/1.1
        Host: {{Hostname}}
        Content-Type: application/json
        
        {"param":"{{payload}}"}

    payloads:
      payload:
        - '"; whoami #'
        - '`whoami`'
    
    matchers-condition: and
    matchers:
      - type: status
        status:
          - 200
      
      - type: regex
        regex:
          - "root|administrator|www-data"
      
      - type: word
        words:
          - "uid="
        part: body

    extractors:
      - type: regex
        name: output
        group: 1
        regex:
          - "uid=([0-9]+)"
```

### Template with Multiple Requests
```yaml
id: multi-step-check

info:
  name: Multi-Step Vulnerability
  author: pentester
  severity: high

requests:
  # Step 1: Check if vulnerable
  - method: GET
    path:
      - "{{BaseURL}}/check"
    
    matchers:
      - type: word
        words:
          - "vulnerable"
    
    extractors:
      - type: regex
        name: token
        internal: true
        group: 1
        regex:
          - 'token=([a-f0-9]+)'

  # Step 2: Exploit with extracted token
  - method: POST
    path:
      - "{{BaseURL}}/exploit"
    
    body: "token={{token}}&cmd=id"
    
    matchers:
      - type: word
        words:
          - "uid="
```

---

## 🔧 Integration with Other Tools

### With Subfinder
```bash
# Subdomain enumeration + Nuclei scanning
subfinder -d target.com -silent | \
httpx -silent | \
nuclei -t cves/ -severity high,critical
```

### With httpx
```bash
# Check live hosts before scanning
cat domains.txt | \
httpx -silent -title -tech-detect | \
nuclei -t technologies/
```

### With Amass
```bash
# Comprehensive domain recon + scanning
amass enum -d target.com | \
nuclei -t exposures/ -t misconfiguration/
```

### With Burp Suite
```bash
# Export Burp targets and scan
nuclei -l burp-targets.txt -t vulnerabilities/
```

---

## 🎓 Template Management

### Installing Custom Templates
```bash
# Clone custom template repo
git clone https://github.com/user/custom-templates ~/.nuclei-templates/custom/

# Use custom templates
nuclei -u https://example.com -t ~/.nuclei-templates/custom/
```

### Template Statistics
```bash
# List all templates
nuclei -tl

# Count templates by severity
nuclei -tl | grep -i critical | wc -l

# List templates by tag
nuclei -tags cve -tl
```

### Updating Templates
```bash
# Update templates
nuclei -update-templates

# Force update
nuclei -update-templates -force

# Disable automatic updates
nuclei -u https://example.com -update-templates=false
```

---

## 🛡️ Best Practices

### Scanning Strategy
```bash
# 1. Start with critical CVEs
nuclei -l targets.txt -t cves/ -severity critical

# 2. Expand to high severity
nuclei -l targets.txt -t vulnerabilities/ -severity high

# 3. Check for misconfigurations
nuclei -l targets.txt -t misconfiguration/

# 4. Technology fingerprinting
nuclei -l targets.txt -t technologies/
```

### Performance Optimization
```bash
# Fast scan
nuclei -u https://example.com -c 100 -rate-limit 100

# Balanced (default)
nuclei -u https://example.com -c 25 -rate-limit 50

# Slow and stealthy
nuclei -u https://example.com -c 5 -rate-limit 2 -timeout 30
```

### Avoiding Detection
```bash
# Random user agent
nuclei -u https://example.com -random-agent

# Custom headers
nuclei -u https://example.com -H "X-Custom: value"

# Through proxy
nuclei -u https://example.com -proxy http://proxy:8080

# Rate limiting
nuclei -u https://example.com -rate-limit 1
```

---

## 📊 Configuration File

Create `~/.config/nuclei/config.yaml`:

```yaml
# Nuclei Configuration
threads: 25
timeout: 10
retries: 1
rate-limit: 150
severity: critical,high,medium,low,info
templates:
  - cves/
  - vulnerabilities/
  - exposures/

# Rate limit
max-host-error: 30

# Output
markdown-export: "reports/"
json-export: "results/"

# Network
http-proxy: "http://127.0.0.1:8080"
disable-redirects: false
```

---

## 🚨 Common Issues and Solutions

### Issue: Templates Not Found
```bash
# Solution: Update templates
nuclei -update-templates
```

### Issue: Too Many False Positives
```bash
# Solution: Use severity filtering and verify results
nuclei -u https://example.com -severity high,critical
```

### Issue: Rate Limiting/Blocking
```bash
# Solution: Reduce speed and randomize
nuclei -u https://example.com -rate-limit 5 -random-agent
```

### Issue: Timeout Errors
```bash
# Solution: Increase timeout
nuclei -u https://example.com -timeout 30
```

---

## 📖 Learning Resources

- **Official Documentation**: [https://nuclei.projectdiscovery.io/](https://nuclei.projectdiscovery.io/)
- **Template Guide**: [https://nuclei.projectdiscovery.io/templating-guide/](https://nuclei.projectdiscovery.io/templating-guide/)
- **Template Repository**: [https://github.com/projectdiscovery/nuclei-templates](https://github.com/projectdiscovery/nuclei-templates)
- **Discord Community**: ProjectDiscovery Discord
- **YouTube**: ProjectDiscovery channel

---

## ⚖️ Legal and Ethical Use

Nuclei is a powerful vulnerability scanner:
- ✅ Only scan authorized systems
- ✅ Follow bug bounty rules
- ✅ Report findings responsibly
- ✅ Use appropriate rate limiting
- ❌ Don't scan without permission
- ❌ Don't cause service disruption
- ❌ Don't exploit found vulnerabilities without authorization

---

## 🔗 Related Tools

- **Subfinder** - Subdomain discovery
- **httpx** - HTTP toolkit
- **Katana** - Web crawler
- **notify** - Notification system
- **interactsh** - OOB interaction server

---

*Fast, accurate, community-driven vulnerability scanning. Use responsibly and ethically.*
