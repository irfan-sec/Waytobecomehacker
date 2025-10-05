# 🐍 Scripting for Security - Bash & Python

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

### Basic Bash Syntax

**Variables**
```bash
#!/bin/bash

# Variable assignment
target="192.168.1.100"
port=80

# Using variables
echo "Scanning $target on port $port"
nmap -p $port $target

# Command substitution
current_time=$(date +%Y-%m-%d_%H-%M-%S)
output_file="scan_${current_time}.txt"
```

**Conditionals**
```bash
# If statement
if [ -f "results.txt" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi

# Check command success
if ping -c 1 google.com &> /dev/null; then
    echo "Internet connection OK"
else
    echo "No internet connection"
fi

# Multiple conditions
if [ -f "file.txt" ] && [ -r "file.txt" ]; then
    echo "File exists and is readable"
fi
```

**Loops**
```bash
# For loop - iterate over list
for ip in 192.168.1.{1..10}; do
    ping -c 1 $ip &> /dev/null && echo "$ip is up"
done

# For loop - iterate over file
while IFS= read -r domain; do
    echo "Scanning $domain"
    nmap -F $domain
done < domains.txt

# While loop
counter=1
while [ $counter -le 5 ]; do
    echo "Attempt $counter"
    ((counter++))
done
```

**Functions**
```bash
# Define function
scan_port() {
    local host=$1
    local port=$2
    
    if nc -zv $host $port 2>&1 | grep -q "succeeded"; then
        echo "Port $port is open on $host"
        return 0
    else
        echo "Port $port is closed on $host"
        return 1
    fi
}

# Use function
scan_port "192.168.1.1" "80"
scan_port "192.168.1.1" "443"
```

### Security Automation Scripts

**1. Network Scanner**
```bash
#!/bin/bash

# Network scanner script
TARGET_NETWORK="192.168.1.0/24"
OUTPUT_DIR="scan_results"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)

# Create output directory
mkdir -p $OUTPUT_DIR

echo "[*] Starting network scan of $TARGET_NETWORK"
echo "[*] Timestamp: $TIMESTAMP"

# Host discovery
echo "[*] Discovering live hosts..."
nmap -sn $TARGET_NETWORK -oG - | grep "Up" | awk '{print $2}' > $OUTPUT_DIR/live_hosts_$TIMESTAMP.txt

# Port scanning
echo "[*] Scanning ports on live hosts..."
while read -r host; do
    echo "[+] Scanning $host"
    nmap -sV -sC -p- $host -oN $OUTPUT_DIR/${host}_$TIMESTAMP.txt &
done < $OUTPUT_DIR/live_hosts_$TIMESTAMP.txt

wait
echo "[*] Scan complete. Results in $OUTPUT_DIR/"
```

**2. Subdomain Enumerator**
```bash
#!/bin/bash

DOMAIN=$1
WORDLIST="/usr/share/wordlists/subdomains.txt"
OUTPUT="subdomains_$DOMAIN.txt"

if [ -z "$DOMAIN" ]; then
    echo "Usage: $0 <domain>"
    exit 1
fi

echo "[*] Enumerating subdomains for $DOMAIN"

# Check if subdomain exists
while read -r subdomain; do
    full_domain="${subdomain}.${DOMAIN}"
    if host $full_domain &> /dev/null; then
        echo "[+] Found: $full_domain"
        echo $full_domain >> $OUTPUT
    fi
done < $WORDLIST

echo "[*] Results saved to $OUTPUT"
```

**3. Log Parser**
```bash
#!/bin/bash

# Parse auth.log for failed login attempts
LOG_FILE="/var/log/auth.log"
OUTPUT="failed_logins.txt"

echo "[*] Parsing failed login attempts..."

grep "Failed password" $LOG_FILE | \
    awk '{print $(NF-3), $(NF-5)}' | \
    sort | uniq -c | sort -rn | \
    while read count user ip; do
        echo "IP: $ip | User: $user | Attempts: $count"
    done > $OUTPUT

echo "[*] Results saved to $OUTPUT"
```

**4. Service Discovery**
```bash
#!/bin/bash

# Discover common services
TARGET=$1
COMMON_PORTS=(21 22 23 25 80 443 3306 3389 5432 8080)

if [ -z "$TARGET" ]; then
    echo "Usage: $0 <target>"
    exit 1
fi

echo "[*] Scanning common ports on $TARGET"

for port in "${COMMON_PORTS[@]}"; do
    (echo > /dev/tcp/$TARGET/$port) 2>/dev/null && \
        echo "[+] Port $port is open" || \
        echo "[-] Port $port is closed"
done
```

---

## 🐍 Python for Security

### Python Basics for Security

**Essential Libraries**
```python
import requests      # HTTP requests
import socket        # Network programming
import subprocess    # Execute commands
import json          # JSON parsing
import re            # Regular expressions
import base64        # Encoding/decoding
import hashlib       # Hashing
import sys, os       # System operations
```

**HTTP Requests**
```python
import requests

# GET request
response = requests.get('https://api.example.com/data')
print(response.status_code)
print(response.text)

# POST request
data = {'username': 'admin', 'password': 'test'}
response = requests.post('https://example.com/login', data=data)

# With headers
headers = {'User-Agent': 'Mozilla/5.0'}
response = requests.get('https://example.com', headers=headers)

# Handle JSON
response = requests.get('https://api.example.com/users')
users = response.json()
for user in users:
    print(user['name'])
```

**File Operations**
```python
# Read file
with open('wordlist.txt', 'r') as f:
    lines = f.readlines()

# Write file
with open('results.txt', 'w') as f:
    f.write('Result data\n')

# Append to file
with open('log.txt', 'a') as f:
    f.write('New log entry\n')
```

**Regular Expressions**
```python
import re

# Extract emails
text = "Contact: admin@example.com, user@test.com"
emails = re.findall(r'[\w\.-]+@[\w\.-]+\.\w+', text)

# Extract IPs
log = "Connection from 192.168.1.100 on port 80"
ip = re.search(r'\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}', log)
if ip:
    print(ip.group())

# Match patterns
if re.match(r'^admin.*', username):
    print("Username starts with admin")
```

### Security Tools in Python

**1. Port Scanner**
```python
#!/usr/bin/env python3
import socket
import sys
from datetime import datetime

def scan_port(target, port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(1)
        result = sock.connect_ex((target, port))
        sock.close()
        return result == 0
    except:
        return False

def main():
    if len(sys.argv) != 2:
        print("Usage: python3 scanner.py <target>")
        sys.exit(1)
    
    target = sys.argv[1]
    print(f"[*] Scanning {target}")
    print(f"[*] Started at {datetime.now()}")
    
    try:
        # Scan common ports
        for port in range(1, 1025):
            if scan_port(target, port):
                print(f"[+] Port {port} is open")
    
    except KeyboardInterrupt:
        print("\n[!] Scan interrupted")
        sys.exit(0)
    
    print(f"[*] Completed at {datetime.now()}")

if __name__ == "__main__":
    main()
```

**2. Web Directory Bruteforcer**
```python
#!/usr/bin/env python3
import requests
import sys
from threading import Thread
from queue import Queue

def check_path(url, path):
    full_url = f"{url}/{path}"
    try:
        response = requests.get(full_url, timeout=3)
        if response.status_code == 200:
            print(f"[+] Found: {full_url} (Status: {response.status_code})")
            return full_url
    except:
        pass
    return None

def worker(queue, url):
    while not queue.empty():
        path = queue.get()
        check_path(url, path)
        queue.task_done()

def main():
    if len(sys.argv) != 3:
        print("Usage: python3 dirbrute.py <url> <wordlist>")
        sys.exit(1)
    
    url = sys.argv[1].rstrip('/')
    wordlist = sys.argv[2]
    
    print(f"[*] Target: {url}")
    print(f"[*] Wordlist: {wordlist}")
    
    # Load wordlist
    with open(wordlist, 'r') as f:
        paths = [line.strip() for line in f]
    
    # Create queue
    queue = Queue()
    for path in paths:
        queue.put(path)
    
    # Create threads
    threads = []
    for _ in range(10):
        t = Thread(target=worker, args=(queue, url))
        t.start()
        threads.append(t)
    
    # Wait for completion
    queue.join()
    print("[*] Scan complete")

if __name__ == "__main__":
    main()
```

**3. Subdomain Enumerator**
```python
#!/usr/bin/env python3
import socket
import sys

def check_subdomain(subdomain, domain):
    full_domain = f"{subdomain}.{domain}"
    try:
        socket.gethostbyname(full_domain)
        return full_domain
    except:
        return None

def main():
    if len(sys.argv) != 3:
        print("Usage: python3 subenum.py <domain> <wordlist>")
        sys.exit(1)
    
    domain = sys.argv[1]
    wordlist = sys.argv[2]
    
    print(f"[*] Enumerating subdomains for {domain}")
    
    with open(wordlist, 'r') as f:
        subdomains = [line.strip() for line in f]
    
    found = []
    for subdomain in subdomains:
        result = check_subdomain(subdomain, domain)
        if result:
            print(f"[+] Found: {result}")
            found.append(result)
    
    print(f"\n[*] Found {len(found)} subdomains")
    
    # Save results
    with open(f'subdomains_{domain}.txt', 'w') as f:
        f.write('\n'.join(found))

if __name__ == "__main__":
    main()
```

**4. Password Sprayer**
```python
#!/usr/bin/env python3
import requests
import sys
from time import sleep

def try_login(url, username, password):
    data = {
        'username': username,
        'password': password
    }
    
    try:
        response = requests.post(url, data=data, timeout=5)
        # Adjust success detection based on application
        if "Welcome" in response.text or response.status_code == 302:
            return True
    except:
        pass
    return False

def main():
    if len(sys.argv) != 4:
        print("Usage: python3 spray.py <url> <userlist> <password>")
        sys.exit(1)
    
    url = sys.argv[1]
    userlist = sys.argv[2]
    password = sys.argv[3]
    
    print(f"[*] Target: {url}")
    print(f"[*] Password: {password}")
    
    with open(userlist, 'r') as f:
        usernames = [line.strip() for line in f]
    
    for username in usernames:
        print(f"[*] Trying {username}")
        if try_login(url, username, password):
            print(f"[+] Success! {username}:{password}")
        sleep(1)  # Rate limiting

if __name__ == "__main__":
    main()
```

**5. Hash Identifier**
```python
#!/usr/bin/env python3
import re
import sys

HASH_TYPES = {
    32: ['MD5', 'NTLM'],
    40: ['SHA-1'],
    64: ['SHA-256'],
    96: ['SHA-384'],
    128: ['SHA-512']
}

def identify_hash(hash_string):
    length = len(hash_string)
    
    # Check if it's hexadecimal
    if not re.match(r'^[a-fA-F0-9]+$', hash_string):
        return "Not a valid hash (contains non-hex characters)"
    
    if length in HASH_TYPES:
        return f"Possible types: {', '.join(HASH_TYPES[length])}"
    else:
        return "Unknown hash type"

def main():
    if len(sys.argv) != 2:
        print("Usage: python3 hashid.py <hash>")
        sys.exit(1)
    
    hash_string = sys.argv[1]
    print(f"Hash: {hash_string}")
    print(f"Length: {len(hash_string)}")
    print(f"Type: {identify_hash(hash_string)}")

if __name__ == "__main__":
    main()
```

---

## 🔧 Advanced Scripting Techniques

### Parallel Processing

**Bash with xargs**
```bash
# Parallel nmap scans
cat hosts.txt | xargs -P 10 -I {} nmap -F {}

# Parallel curl requests
cat urls.txt | xargs -P 20 -I {} curl -s {}
```

**Python with Threading**
```python
from threading import Thread
from queue import Queue

def worker(queue):
    while not queue.empty():
        item = queue.get()
        # Process item
        queue.task_done()

# Create queue and threads
queue = Queue()
for item in items:
    queue.put(item)

threads = []
for _ in range(10):
    t = Thread(target=worker, args=(queue,))
    t.start()
    threads.append(t)

queue.join()
```

### Error Handling

**Bash**
```bash
# Exit on error
set -e

# Check command success
if ! command; then
    echo "Command failed"
    exit 1
fi

# Trap errors
trap 'echo "Error on line $LINENO"' ERR
```

**Python**
```python
try:
    response = requests.get(url, timeout=5)
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.ConnectionError:
    print("Connection error")
except Exception as e:
    print(f"Error: {e}")
```

### Logging

**Python Logging**
```python
import logging

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('security_scan.log'),
        logging.StreamHandler()
    ]
)

# Use logging
logging.info('Starting scan')
logging.warning('Potential vulnerability found')
logging.error('Scan failed')
```

---

## 📚 Useful One-Liners

### Bash One-Liners

```bash
# Extract URLs from file
grep -oP 'https?://[^"]+' file.html

# Find live hosts
for ip in 192.168.1.{1..254}; do ping -c 1 $ip &>/dev/null && echo "$ip is up"; done

# Check if port is open
nc -zv host port 2>&1 | grep succeeded

# Extract emails from text
grep -oE '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' file.txt

# Decode base64
echo "base64string" | base64 -d

# Generate random password
openssl rand -base64 32
```

### Python One-Liners

```python
# HTTP server
python3 -m http.server 8000

# Check if port is open
python3 -c "import socket; s=socket.socket(); s.settimeout(1); print(s.connect_ex(('host', 80)) == 0)"

# Decode base64
python3 -c "import base64; print(base64.b64decode('string'))"

# Calculate hash
python3 -c "import hashlib; print(hashlib.sha256(b'text').hexdigest())"

# URL encode
python3 -c "from urllib.parse import quote; print(quote('string'))"
```

---

## 🎯 Practice Projects

1. **Automated Recon Tool**
   - Subdomain enumeration
   - Port scanning
   - Technology detection
   - Report generation

2. **Log Analyzer**
   - Parse auth logs
   - Identify suspicious activity
   - Generate alerts
   - Visualize data

3. **Vulnerability Scanner**
   - Check for common vulns
   - Test authentication
   - Scan for misconfigurations
   - Generate report

4. **Password Auditor**
   - Check password strength
   - Compare against leaked databases
   - Generate secure passwords
   - Policy enforcement

---

## 📖 Learning Resources

### Bash
- **Linux Command Line** by William Shotts
- **Bash Cookbook** by O'Reilly
- **Explainshell**: [https://explainshell.com](https://explainshell.com)

### Python
- **Automate the Boring Stuff with Python**
- **Black Hat Python** by Justin Seitz
- **Violent Python** by TJ O'Connor
- **Python for Cybersecurity** courses

### Practice
- **TryHackMe**: Scripting rooms
- **HackTheBox**: Automation challenges
- **GitHub**: Study security scripts

---

*Automate intelligently, script efficiently, hack productively.*
