---
title: "Scripting for Security - Bash & Python"
permalink: /Scripting/
layout: single
author_profile: true
toc: true
toc_sticky: true
---

> Automate security tasks with Bash and Python

---

## 📋 Overview

Scripting is an essential skill for cybersecurity professionals. Whether you're automating reconnaissance, parsing log files, or building custom exploits, knowing Bash and Python will significantly enhance your efficiency.

**Why Learn Scripting?**
- ⚡ Automate repetitive tasks
- 🔧 Create custom security tools
- 📊 Parse and analyze data efficiently
- 🎯 Build exploit scripts
- 🚀 Speed up reconnaissance workflows

---

## 🐚 Bash Scripting for Security

### Why Bash?
- Pre-installed on all Linux systems
- Perfect for system administration tasks
- Excellent for chaining commands
- Quick automation of command-line tools

### Basic Bash for Security

**Port Scanner Script**
```bash
#!/bin/bash

target=$1
echo "Scanning $target..."

for port in {1..1000}; do
    timeout 1 bash -c "echo >/dev/tcp/$target/$port" 2>/dev/null && 
    echo "Port $port is open"
done
```

**Subdomain Enumeration**
```bash
#!/bin/bash

domain=$1
wordlist="subdomains.txt"

while IFS= read -r subdomain; do
    if host "$subdomain.$domain" > /dev/null 2>&1; then
        echo "[+] Found: $subdomain.$domain"
    fi
done < "$wordlist"
```

**Log Analysis**
```bash
#!/bin/bash

# Find failed SSH login attempts
grep "Failed password" /var/log/auth.log | 
awk '{print $(NF-3)}' | 
sort | uniq -c | sort -nr | head -10

# Find most accessed URLs
awk '{print $7}' /var/log/apache2/access.log | 
sort | uniq -c | sort -nr | head -20
```

---

## 🐍 Python for Security

### Why Python?
- Extensive security libraries
- Easy to read and write
- Great for network programming
- Platform independent
- Active security community

### Essential Python Libraries

**Security-Focused Libraries**
```python
import requests      # HTTP requests
import socket        # Network programming
import paramiko      # SSH connections
import scapy         # Packet manipulation
import subprocess    # Run system commands
import hashlib       # Hashing functions
import base64        # Encoding/decoding
import re           # Regular expressions
```

### Python Security Scripts

**Port Scanner**
```python
#!/usr/bin/env python3
import socket
from concurrent.futures import ThreadPoolExecutor

def scan_port(host, port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(1)
        result = sock.connect_ex((host, port))
        sock.close()
        if result == 0:
            return port
    except:
        pass
    return None

def scan_host(host, ports):
    print(f"Scanning {host}...")
    open_ports = []
    
    with ThreadPoolExecutor(max_workers=100) as executor:
        results = executor.map(lambda p: scan_port(host, p), ports)
        open_ports = [p for p in results if p]
    
    return open_ports

if __name__ == "__main__":
    target = "192.168.1.1"
    ports = range(1, 1001)
    open_ports = scan_host(target, ports)
    
    print(f"\nOpen ports on {target}:")
    for port in open_ports:
        print(f"  Port {port} is open")
```

**Directory Brute Force**
```python
#!/usr/bin/env python3
import requests
import sys
from concurrent.futures import ThreadPoolExecutor

def check_path(base_url, path):
    url = f"{base_url}/{path}"
    try:
        response = requests.get(url, timeout=3, allow_redirects=False)
        if response.status_code == 200:
            return (path, response.status_code, len(response.content))
    except:
        pass
    return None

def brute_force_dirs(base_url, wordlist):
    print(f"Scanning {base_url}...")
    
    with open(wordlist, 'r') as f:
        paths = [line.strip() for line in f]
    
    with ThreadPoolExecutor(max_workers=50) as executor:
        results = executor.map(lambda p: check_path(base_url, p), paths)
        
    found = [r for r in results if r]
    return found

if __name__ == "__main__":
    if len(sys.argv) < 3:
        print("Usage: python3 dirbrute.py <url> <wordlist>")
        sys.exit(1)
    
    url = sys.argv[1]
    wordlist = sys.argv[2]
    
    results = brute_force_dirs(url, wordlist)
    
    print("\n[+] Found:")
    for path, status, size in results:
        print(f"  /{path} - Status: {status}, Size: {size}")
```

**Hash Cracker**
```python
#!/usr/bin/env python3
import hashlib
import sys

def crack_hash(hash_to_crack, wordlist, hash_type='md5'):
    print(f"Cracking {hash_type} hash: {hash_to_crack}")
    
    with open(wordlist, 'r', encoding='latin-1') as f:
        for line in f:
            password = line.strip()
            
            if hash_type == 'md5':
                hash_obj = hashlib.md5(password.encode())
            elif hash_type == 'sha1':
                hash_obj = hashlib.sha1(password.encode())
            elif hash_type == 'sha256':
                hash_obj = hashlib.sha256(password.encode())
            
            hashed = hash_obj.hexdigest()
            
            if hashed == hash_to_crack:
                return password
    
    return None

if __name__ == "__main__":
    if len(sys.argv) < 3:
        print("Usage: python3 hashcrack.py <hash> <wordlist> [hash_type]")
        sys.exit(1)
    
    target_hash = sys.argv[1]
    wordlist = sys.argv[2]
    hash_type = sys.argv[3] if len(sys.argv) > 3 else 'md5'
    
    result = crack_hash(target_hash, wordlist, hash_type)
    
    if result:
        print(f"[+] Password found: {result}")
    else:
        print("[-] Password not found in wordlist")
```

**Web Vulnerability Scanner**
```python
#!/usr/bin/env python3
import requests
from urllib.parse import urljoin

class VulnScanner:
    def __init__(self, base_url):
        self.base_url = base_url
        self.session = requests.Session()
    
    def test_sql_injection(self, url):
        """Test for SQL injection"""
        payloads = ["'", "1' OR '1'='1", "1' OR '1'='1' --"]
        vulnerabilities = []
        
        for payload in payloads:
            test_url = f"{url}?id={payload}"
            try:
                response = self.session.get(test_url)
                if "sql" in response.text.lower() or "mysql" in response.text.lower():
                    vulnerabilities.append(("SQL Injection", payload))
            except:
                pass
        
        return vulnerabilities
    
    def test_xss(self, url):
        """Test for XSS"""
        payloads = [
            "<script>alert('XSS')</script>",
            "<img src=x onerror=alert('XSS')>",
            "javascript:alert('XSS')"
        ]
        vulnerabilities = []
        
        for payload in payloads:
            test_url = f"{url}?search={payload}"
            try:
                response = self.session.get(test_url)
                if payload in response.text:
                    vulnerabilities.append(("XSS", payload))
            except:
                pass
        
        return vulnerabilities
    
    def scan(self):
        print(f"Scanning {self.base_url}...")
        
        # Test SQL injection
        sql_vulns = self.test_sql_injection(self.base_url)
        if sql_vulns:
            print("\n[!] SQL Injection vulnerabilities found:")
            for vuln_type, payload in sql_vulns:
                print(f"  - Payload: {payload}")
        
        # Test XSS
        xss_vulns = self.test_xss(self.base_url)
        if xss_vulns:
            print("\n[!] XSS vulnerabilities found:")
            for vuln_type, payload in xss_vulns:
                print(f"  - Payload: {payload}")

if __name__ == "__main__":
    import sys
    if len(sys.argv) < 2:
        print("Usage: python3 vulnscan.py <url>")
        sys.exit(1)
    
    scanner = VulnScanner(sys.argv[1])
    scanner.scan()
```

---

## 🛠️ Building Your Security Toolkit

### Project Ideas

**Beginner Projects**
1. **Password Generator** - Create strong passwords
2. **Hash Calculator** - Hash files and strings
3. **Banner Grabber** - Identify service versions
4. **Ping Sweeper** - Find live hosts

**Intermediate Projects**
1. **Network Scanner** - Complete port and service scanner
2. **Web Crawler** - Discover web application structure
3. **Log Parser** - Analyze security logs
4. **Exploit Framework** - Build your own framework

**Advanced Projects**
1. **Custom Fuzzer** - Application fuzzing tool
2. **C2 Framework** - Command and control system
3. **IDS/IPS** - Intrusion detection/prevention
4. **Packet Analyzer** - Custom packet inspection tool

---

## 📚 Learning Path

### Month 1: Bash Basics
- [ ] Master basic Bash syntax
- [ ] Learn file operations and text processing
- [ ] Create simple automation scripts
- [ ] Practice with Linux commands

### Month 2: Python Fundamentals
- [ ] Learn Python basics (variables, loops, functions)
- [ ] Understand file I/O
- [ ] Work with libraries (requests, socket)
- [ ] Build simple security tools

### Month 3: Advanced Scripting
- [ ] Multi-threading and async programming
- [ ] Network programming
- [ ] API interactions
- [ ] Build complex security tools

### Month 4+: Specialization
- [ ] Web application testing scripts
- [ ] Network exploitation tools
- [ ] Custom exploit development
- [ ] Contribute to open-source projects

---

## 🎓 Resources

### Online Courses
- **[Automate the Boring Stuff with Python](https://automatetheboringstuff.com/)** - Free Python book
- **[Bash Scripting Tutorial](https://www.shellscript.sh/)** - Comprehensive Bash guide
- **[TryHackMe](https://tryhackme.com/)** - Scripting rooms

### Books
- "Black Hat Python" by Justin Seitz
- "Violent Python" by TJ O'Connor
- "Gray Hat Python" by Justin Seitz

### Practice
- [HackerRank](https://www.hackerrank.com/) - Python challenges
- [Exercism](https://exercism.org/) - Coding practice
- [Project Euler](https://projecteuler.net/) - Math problems

---

## 🔗 Next Steps

After mastering scripting basics:
1. **[Web Hacking Tools](../Web-Hacking-Tools/)** - Learn existing tools
2. **[Networking](../Networking/)** - Understand network protocols
3. **[Exploitation](../Exploitation/)** - Apply scripts to exploitation
4. **[CTF](../CTF/)** - Use scripts in competitions

---

## 📖 Detailed Guide

For comprehensive scripting examples and projects:

**[View Complete Bash & Python Security Guide](./Bash-Python-Security/)**

---

<div class="notice--info">
  <h4>💡 Pro Tip</h4>
  <p>The best way to learn scripting is by solving real problems. Start with simple automation tasks, then gradually build more complex tools. Don't just copy scripts—understand how they work and modify them for your needs.</p>
</div>

---

*"Automation is not about replacing humans; it's about freeing them to do more creative work."* 🚀
