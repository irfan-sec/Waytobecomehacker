# 🚀 ffuf - Fast Web Fuzzer

> Lightning-fast web fuzzing tool written in Go for content discovery and testing

---

## 📋 Overview

**ffuf** (Fuzz Faster U Fool) is a fast web fuzzer designed for discovering hidden content, testing parameters, and fuzzing various parts of web applications. Written in Go, it's incredibly fast and supports multiple fuzzing modes including directory brute-forcing, parameter fuzzing, and virtual host discovery.

**Key Features:**
- ⚡ Extremely fast performance (Go-based)
- 🎯 Multiple fuzzing modes (directories, parameters, headers, etc.)
- 📊 Flexible matching and filtering options
- 🔄 Recursion support for deep directory discovery
- 📁 Multiple output formats (JSON, CSV, HTML)
- 🎨 Colorized output for better readability

---

## 🛠️ Installation

### On Kali Linux
```bash
sudo apt update
sudo apt install ffuf -y
```

### Using Go
```bash
go install github.com/ffuf/ffuf@latest
```

### From Source
```bash
git clone https://github.com/ffuf/ffuf
cd ffuf
go build
sudo mv ffuf /usr/local/bin/
```

### Verify Installation
```bash
ffuf -V
```

---

## 📚 Basic Usage

### Directory Discovery
```bash
# Basic directory fuzzing
ffuf -u http://target.com/FUZZ -w /usr/share/wordlists/dirb/common.txt

# With file extensions
ffuf -u http://target.com/FUZZ -w wordlist.txt -e .php,.html,.txt,.bak

# Recursive fuzzing
ffuf -u http://target.com/FUZZ -w wordlist.txt -recursion -recursion-depth 2
```

### Parameter Fuzzing
```bash
# GET parameter fuzzing
ffuf -u http://target.com/page.php?FUZZ=value -w params.txt

# POST parameter fuzzing
ffuf -u http://target.com/login -w params.txt -X POST -d "FUZZ=test"

# Multiple parameters
ffuf -u http://target.com/api?param1=FUZZ1&param2=FUZZ2 -w wordlist.txt:FUZZ1,FUZZ2
```

### Virtual Host Discovery
```bash
# Subdomain/vhost fuzzing
ffuf -u http://target.com -H "Host: FUZZ.target.com" -w subdomains.txt

# Filter by size
ffuf -u http://target.com -H "Host: FUZZ.target.com" -w subdomains.txt -fs 1234
```

---

## 🎯 Advanced Techniques

### Filtering Results
```bash
# Filter by status code
ffuf -u http://target.com/FUZZ -w wordlist.txt -fc 404,403

# Filter by response size
ffuf -u http://target.com/FUZZ -w wordlist.txt -fs 1234

# Filter by word count
ffuf -u http://target.com/FUZZ -w wordlist.txt -fw 100

# Filter by line count
ffuf -u http://target.com/FUZZ -w wordlist.txt -fl 50

# Filter by regex
ffuf -u http://target.com/FUZZ -w wordlist.txt -fr "error"
```

### Matching Specific Results
```bash
# Match specific status codes
ffuf -u http://target.com/FUZZ -w wordlist.txt -mc 200,301

# Match by size
ffuf -u http://target.com/FUZZ -w wordlist.txt -ms 1234

# Match by words
ffuf -u http://target.com/FUZZ -w wordlist.txt -mw 100
```

### Speed and Performance
```bash
# Increase threads (default 40)
ffuf -u http://target.com/FUZZ -w wordlist.txt -t 100

# Add delay between requests
ffuf -u http://target.com/FUZZ -w wordlist.txt -p 0.5

# Rate limiting (requests per second)
ffuf -u http://target.com/FUZZ -w wordlist.txt -rate 100
```

### Authentication
```bash
# Basic authentication
ffuf -u http://target.com/FUZZ -w wordlist.txt -H "Authorization: Basic dXNlcjpwYXNz"

# Cookie-based auth
ffuf -u http://target.com/FUZZ -w wordlist.txt -b "session=abc123"

# Custom headers
ffuf -u http://target.com/FUZZ -w wordlist.txt -H "X-Custom-Header: value"
```

---

## 💡 Real-World Scenarios

### Scenario 1: Admin Panel Discovery
```bash
# Search for admin interfaces
ffuf -u http://target.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/admin-panels.txt \
     -mc 200,301,302 -fc 404 -c
```

### Scenario 2: API Endpoint Discovery
```bash
# Find API endpoints
ffuf -u http://target.com/api/FUZZ -w api-endpoints.txt \
     -H "Authorization: Bearer token" \
     -mc 200,201 -c
```

### Scenario 3: Backup File Discovery
```bash
# Find backup files
ffuf -u http://target.com/FUZZ -w wordlist.txt \
     -e .bak,.old,.backup,.zip,.tar.gz \
     -fc 404 -c
```

### Scenario 4: Parameter Fuzzing for SQLi
```bash
# Fuzz parameters looking for SQL injection
ffuf -u http://target.com/page.php?id=FUZZ -w /usr/share/seclists/Fuzzing/SQLi/quick-SQLi.txt \
     -mr "error|sql|mysql|syntax" -c
```

### Scenario 5: Multi-Wordlist Fuzzing
```bash
# Use multiple wordlists
ffuf -u http://target.com/FUZZ1/FUZZ2 \
     -w dirs.txt:FUZZ1 \
     -w files.txt:FUZZ2 \
     -mc 200 -c
```

---

## 🎓 Output and Reporting

### Save Results
```bash
# JSON output
ffuf -u http://target.com/FUZZ -w wordlist.txt -o results.json -of json

# CSV output
ffuf -u http://target.com/FUZZ -w wordlist.txt -o results.csv -of csv

# HTML report
ffuf -u http://target.com/FUZZ -w wordlist.txt -o results.html -of html

# All formats
ffuf -u http://target.com/FUZZ -w wordlist.txt -o results -of all
```

### Silent and Verbose Modes
```bash
# Silent mode (only show results)
ffuf -u http://target.com/FUZZ -w wordlist.txt -s

# Verbose mode (show all details)
ffuf -u http://target.com/FUZZ -w wordlist.txt -v
```

---

## 🔧 Configuration File

Create a config file `~/.ffufrc`:

```ini
[http]
    headers = ["User-Agent: ffuf"]
    
[general]
    colors = true
    delay = 0
    maxtime = 0
    maxtime-job = 0
    quiet = false
    rate = 0
    stopon403 = false
    stopon429 = false
    stoponerrors = false
    threads = 40
    verbose = false

[output]
    debuglog = ""
    outputdirectory = ""
    outputformat = "json"
```

---

## 🚨 Tips and Best Practices

### Performance Optimization
```bash
# Fast scan with auto-calibration
ffuf -u http://target.com/FUZZ -w wordlist.txt -ac -t 100

# Smart filtering (auto-detect false positives)
ffuf -u http://target.com/FUZZ -w wordlist.txt -ac
```

### Common Wordlists
```bash
# SecLists (must have)
git clone https://github.com/danielmiessler/SecLists.git

# Common wordlists locations
/usr/share/wordlists/dirb/common.txt
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
/usr/share/seclists/Discovery/Web-Content/
```

### Combining with Other Tools
```bash
# Pipe to other tools
ffuf -u http://target.com/FUZZ -w wordlist.txt -mc 200 -s | \
     awk '{print $1}' | \
     httpx -silent

# Use with Burp Suite
ffuf -u http://target.com/FUZZ -w wordlist.txt -x http://127.0.0.1:8080
```

---

## ⚠️ Common Pitfalls

1. **WAF Detection**: Use delays and custom headers to avoid WAF blocking
2. **Rate Limiting**: Respect rate limits with `-rate` flag
3. **False Positives**: Use auto-calibration `-ac` or manual filtering
4. **Large Wordlists**: Start with smaller lists, then expand
5. **Network Issues**: Use `-timeout` to handle slow responses

---

## 📖 Learning Resources

- **Official Documentation**: [https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf)
- **ffuf Wiki**: [https://github.com/ffuf/ffuf/wiki](https://github.com/ffuf/ffuf/wiki)
- **TryHackMe Rooms**: Content Discovery, Web Enumeration
- **HackTricks ffuf**: [https://book.hacktricks.xyz/pentesting-web/web-tool-ffuf](https://book.hacktricks.xyz/pentesting-web/web-tool-ffuf)

---

## ⚖️ Legal Notice

**ffuf** is a powerful tool for security testing. Always ensure you have:
- ✅ Written permission to test the target
- ✅ Clear scope definition
- ✅ Understanding of legal implications
- ❌ Never use on unauthorized systems

---

## 🔗 Related Tools

- **Gobuster** - Alternative directory brute-forcer
- **Dirsearch** - Python-based web path scanner
- **wfuzz** - Web application fuzzer
- **Feroxbuster** - Rust-based content discovery

---

*Fast fuzzing for faster findings. Use responsibly and ethically.*
