# 🔍 Gobuster - Ultimate Directory Discovery Tool

> Fast and flexible directory/file/subdomain brute forcer written in Go

---

## 📌 What is Gobuster?

**Gobuster** is a tool used to brute-force directories, files, and subdomains on web servers. It's written in Go, making it extremely fast and efficient for enumeration tasks during penetration testing and security assessments.

### Why Gobuster?
- ⚡ **Blazing Fast** - Written in Go with concurrent goroutines
- 🎯 **Multiple Modes** - Directory, file, subdomain, and virtual host discovery
- 🔧 **Highly Configurable** - Extensive options for customization
- 📝 **Clean Output** - Easy to parse results
- 🚀 **Active Development** - Regularly updated with new features

---

## 🚀 Installation

### Kali Linux (Pre-installed)
```bash
# Gobuster comes pre-installed on Kali Linux
gobuster --help
```

### Ubuntu/Debian
```bash
sudo apt update
sudo apt install gobuster
```

### From Source (Latest Version)
```bash
# Install Go first, then:
go install github.com/OJ/gobuster/v3@latest
```

### Download Binary
```bash
# Download from GitHub releases
wget https://github.com/OJ/gobuster/releases/download/v3.6.0/gobuster_Linux_x86_64.tar.gz
tar -xzf gobuster_Linux_x86_64.tar.gz
```

---

## 🎯 Basic Usage

### Directory Discovery
```bash
# Basic directory brute force
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt

# With file extensions
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt -x php,html,txt,js

# Show response length
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt -l
```

### File Discovery
```bash
# Look for specific files
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt -x php,txt,bak,old

# Search for backup files
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt -x bak,backup,old,tmp
```

### Subdomain Discovery
```bash
# Enumerate subdomains
gobuster dns -d target.com -w /usr/share/wordlists/amass/subdomains-top1mil-5000.txt

# With custom resolver
gobuster dns -d target.com -w wordlist.txt -r 8.8.8.8,1.1.1.1
```

---

## 🔧 Advanced Techniques

### 1. Custom Headers & Authentication
```bash
# With custom headers
gobuster dir -u http://target.com -w wordlist.txt -H "Authorization: Bearer token123"

# With cookies
gobuster dir -u http://target.com -w wordlist.txt -c "session=abc123; auth=xyz789"

# With User-Agent
gobuster dir -u http://target.com -w wordlist.txt -a "Custom-Agent/1.0"
```

### 2. Status Code Filtering
```bash
# Include specific status codes
gobuster dir -u http://target.com -w wordlist.txt -s "200,204,301,302,307,401,403"

# Exclude status codes
gobuster dir -u http://target.com -w wordlist.txt -b "404,400"

# Show all status codes
gobuster dir -u http://target.com -w wordlist.txt -s "100-599"
```

### 3. Response Analysis
```bash
# Show response length
gobuster dir -u http://target.com -w wordlist.txt -l

# Show full URLs
gobuster dir -u http://target.com -w wordlist.txt --expanded

# Follow redirects
gobuster dir -u http://target.com -w wordlist.txt -r
```

### 4. Performance Tuning
```bash
# Increase threads (default: 10)
gobuster dir -u http://target.com -w wordlist.txt -t 50

# Add delay between requests (milliseconds)
gobuster dir -u http://target.com -w wordlist.txt --delay 100ms

# Set timeout (default: 7s)
gobuster dir -u http://target.com -w wordlist.txt --timeout 10s
```

---

## 📊 Different Modes Explained

### 1. DIR Mode (Directory/File Discovery)
```bash
# Standard directory enumeration
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/big.txt

# Options:
# -u: Target URL
# -w: Wordlist path
# -x: File extensions to search for
# -l: Include length of response body
# -r: Follow redirects
# -s: Positive status codes
# -b: Negative status codes
```

### 2. DNS Mode (Subdomain Discovery)
```bash
# Subdomain enumeration
gobuster dns -d target.com -w /usr/share/wordlists/amass/subdomains-top1mil-20000.txt

# Options:
# -d: Domain name to enumerate
# -w: Wordlist path
# -r: Custom DNS resolver
# -i: Show IP addresses
# -c: Show CNAME records
```

### 3. VHOST Mode (Virtual Host Discovery)
```bash
# Virtual host enumeration
gobuster vhost -u http://target.com -w wordlist.txt

# Options:
# -u: Base URL
# -w: Wordlist path
# --append-domain: Append domain to wordlist entries
```

### 4. FUZZ Mode (General Purpose Fuzzing)
```bash
# Fuzz any part of the URL
gobuster fuzz -u http://target.com/FUZZ -w wordlist.txt

# Multiple FUZZ positions
gobuster fuzz -u http://target.com/FUZZ/FUZZ2 -w wordlist1.txt:FUZZ -w wordlist2.txt:FUZZ2
```

---

## 📝 Essential Wordlists

### Default Locations (Kali Linux)
```bash
# Common directories
/usr/share/wordlists/dirb/common.txt
/usr/share/wordlists/dirb/big.txt

# Web content
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt

# Subdomains
/usr/share/wordlists/amass/subdomains-top1mil-5000.txt
/usr/share/wordlists/amass/subdomains-top1mil-20000.txt
```

### Recommended External Wordlists
```bash
# SecLists (Download separately)
git clone https://github.com/danielmiessler/SecLists.git

# Popular choices:
# SecLists/Discovery/Web-Content/directory-list-2.3-big.txt
# SecLists/Discovery/Web-Content/raft-large-directories.txt
# SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

### Creating Custom Wordlists
```bash
# Extract words from target website
cewl http://target.com -w custom-wordlist.txt

# Combine multiple wordlists
cat wordlist1.txt wordlist2.txt | sort -u > combined.txt

# Generate permutations
echo -e "admin\nlogin\ntest" | sed 's/$/-old/' > custom.txt
```

---

## 💡 Real-World Scenarios

### 1. Initial Web App Enumeration
```bash
# Step 1: Basic directory discovery
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt -x php,html,txt

# Step 2: Deeper enumeration on found directories
gobuster dir -u http://target.com/admin -w /usr/share/wordlists/dirb/big.txt -x php

# Step 3: Look for backup files
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt -x bak,backup,old,tmp
```

### 2. API Endpoint Discovery
```bash
# Look for API endpoints
gobuster dir -u http://target.com -w api-wordlist.txt -x json

# Common API paths
echo -e "api\nv1\nv2\nrest\ngraphql\nendpoint" > api-wordlist.txt
gobuster dir -u http://target.com -w api-wordlist.txt
```

### 3. Subdomain Enumeration
```bash
# Comprehensive subdomain discovery
gobuster dns -d target.com -w /usr/share/wordlists/amass/subdomains-top1mil-20000.txt -i

# Check for wildcard DNS
dig *.target.com @8.8.8.8
```

### 4. Virtual Host Discovery
```bash
# Find virtual hosts on same IP
gobuster vhost -u http://target-ip -w /usr/share/wordlists/dirb/common.txt --append-domain

# Test with custom domain
gobuster vhost -u http://192.168.1.100 -w wordlist.txt -H "Host: FUZZ.target.com"
```

---

## 🎯 Pro Tips & Best Practices

### 1. Optimize Performance
```bash
# For internal networks (faster)
gobuster dir -u http://internal-target -w wordlist.txt -t 100 --timeout 3s

# For external targets (respectful)
gobuster dir -u http://external-target -w wordlist.txt -t 10 --delay 100ms
```

### 2. Handle Rate Limiting
```bash
# Add delays and reduce threads
gobuster dir -u http://target.com -w wordlist.txt -t 5 --delay 500ms

# Use random User-Agent
gobuster dir -u http://target.com -w wordlist.txt --random-agent
```

### 3. Output Management
```bash
# Save results to file
gobuster dir -u http://target.com -w wordlist.txt -o results.txt

# Verbose output for debugging
gobuster dir -u http://target.com -w wordlist.txt -v

# Quiet mode (minimal output)
gobuster dir -u http://target.com -w wordlist.txt -q
```

### 4. Combine with Other Tools
```bash
# Pipe results to other tools
gobuster dir -u http://target.com -w wordlist.txt -q | grep "Status: 200" | awk '{print $1}' | httpx

# Use with curl for further testing
gobuster dir -u http://target.com -w wordlist.txt -q | while read url; do curl -I "$url"; done
```

---

## 🛡️ Evasion Techniques

### 1. Bypass WAF/Rate Limiting
```bash
# Random User-Agent
gobuster dir -u http://target.com -w wordlist.txt --random-agent

# Custom headers to appear legitimate
gobuster dir -u http://target.com -w wordlist.txt -H "X-Originating-IP: 127.0.0.1" -H "X-Forwarded-For: 127.0.0.1"

# Slower, stealthier scanning
gobuster dir -u http://target.com -w wordlist.txt -t 1 --delay 2s
```

### 2. Certificate Issues
```bash
# Skip SSL certificate verification
gobuster dir -u https://target.com -w wordlist.txt -k

# Use specific proxy
gobuster dir -u http://target.com -w wordlist.txt -p http://proxy:8080
```

---

## 📚 Learning Exercises

### Beginner Level:
1. **Setup Practice**: Install gobuster and practice on DVWA
2. **Basic Enumeration**: Find hidden directories on a test application
3. **Extension Discovery**: Look for different file types (.php, .txt, .bak)
4. **Status Code Analysis**: Understand different HTTP response codes

### Intermediate Level:
1. **Wordlist Optimization**: Create custom wordlists for specific targets
2. **Subdomain Discovery**: Find subdomains of a target domain
3. **Performance Tuning**: Optimize gobuster for different network conditions
4. **Output Processing**: Process gobuster results with other tools

### Advanced Level:
1. **Evasion Techniques**: Bypass rate limiting and WAF protection
2. **Automation**: Write scripts to automate gobuster workflows
3. **Integration**: Combine gobuster with other reconnaissance tools
4. **Custom Modes**: Use FUZZ mode for specialized enumeration

---

## 🔗 Useful Commands Cheat Sheet

```bash
# Quick directory scan
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt

# Comprehensive file discovery
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/big.txt -x php,html,txt,js,xml,json

# Subdomain enumeration
gobuster dns -d target.com -w /usr/share/wordlists/amass/subdomains-top1mil-5000.txt

# Virtual host discovery
gobuster vhost -u http://target.com -w /usr/share/wordlists/dirb/common.txt

# API endpoint discovery
gobuster dir -u http://target.com -w api-wordlist.txt -x json,xml

# Backup file hunting
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt -x bak,backup,old,tmp,swp

# Silent scan with output
gobuster dir -u http://target.com -w wordlist.txt -q -o results.txt

# High-performance internal scan
gobuster dir -u http://internal-target -w wordlist.txt -t 50 --timeout 3s
```

---

## ⚠️ Ethical Considerations

### ✅ Authorized Testing Only
- Only scan systems you own or have explicit permission to test
- Respect rate limits and server resources
- Follow responsible disclosure practices

### 🚫 Avoid These Mistakes
- Don't run aggressive scans on production systems without permission
- Don't ignore robots.txt and security policies
- Don't use gobuster for malicious purposes

---

<div class="notice--success">
  <h4>💡 Pro Tip</h4>
  <p>Gobuster is most effective when combined with manual testing and other reconnaissance tools. Use it to discover endpoints, then investigate findings manually for potential vulnerabilities.</p>
</div>

---

*Gobuster is an essential tool in every penetration tester's toolkit. Master its various modes and options to become more efficient at web application enumeration.*