# 🚩 Capture The Flag (CTF) Guide

> Complete guide to competing in CTF competitions and hacking challenges

---

## 📋 Overview

**Capture The Flag (CTF)** competitions are cybersecurity challenges where participants solve security-related tasks to find hidden flags. They're excellent for developing practical hacking skills, learning new techniques, and preparing for real-world pentesting scenarios.

**Why CTFs?**
- 🎯 Hands-on practical experience
- 🏆 Competitive learning environment
- 🧠 Problem-solving skills
- 🤝 Networking with security community
- 💼 Resume/portfolio building
- 🔍 Discover new techniques and tools

---

## 🎮 Types of CTF Competitions

### 1. Jeopardy Style
Most common format with categories and point-based challenges.

**Categories:**
- **Web** - Web application vulnerabilities
- **Crypto** - Cryptography challenges
- **Pwn/Binary** - Binary exploitation
- **Reverse Engineering** - Analyzing compiled programs
- **Forensics** - Digital forensics and file analysis
- **OSINT** - Open Source Intelligence gathering
- **Misc** - Anything else (steganography, networking, etc.)

### 2. Attack-Defense
Teams defend their own servers while attacking others.

### 3. King of the Hill
Competitive format where teams fight for control of a system.

### 4. Boot2Root
Realistic scenarios where you compromise a machine from nothing to root/admin.

---

## 🔧 Essential Tools for CTFs

### General Tools
```bash
# Multi-tool frameworks
pwntools          # Python exploit development
pwndbg           # GDB enhancement
radare2          # Reverse engineering
ghidra           # NSA's reverse engineering tool
```

### Web Challenges
```bash
# Web testing
burp suite       # Web proxy and testing
curl / wget      # Command-line web requests
sqlmap           # SQL injection
dirb / gobuster  # Directory bruteforcing
nikto            # Web scanner
```

### Cryptography
```bash
# Crypto tools
hashcat          # Password cracking
john             # John the Ripper
openssl          # SSL/TLS toolkit
sage             # Mathematical software
RSActftool       # RSA attacks
```

### Binary Exploitation
```bash
# Binary analysis
gdb              # GNU debugger
objdump          # Object file viewer
strings          # Extract strings from binary
strace           # Trace system calls
ltrace           # Trace library calls
```

### Reverse Engineering
```bash
# RE tools
ida pro / ida free    # Interactive disassembler
ghidra                # Free RE tool by NSA
radare2               # Open source RE framework
hopper                # macOS/Linux disassembler
binary ninja          # Modern RE platform
```

### Forensics
```bash
# Forensics tools
volatility       # Memory forensics
autopsy          # Digital forensics
binwalk          # Firmware analysis
exiftool         # Metadata analysis
foremost         # File carving
```

### Steganography
```bash
# Stego tools
steghide         # Hide/extract data
stegsolve        # Image analysis
zsteg            # PNG/BMP analysis
sonic-visualizer # Audio analysis
```

---

## 🎯 Category-Specific Strategies

### Web Exploitation

**Common Vulnerabilities:**
```
- SQL Injection
- XSS (Cross-Site Scripting)
- CSRF (Cross-Site Request Forgery)
- SSRF (Server-Side Request Forgery)
- LFI/RFI (Local/Remote File Inclusion)
- Authentication bypass
- Command injection
```

**Quick Wins:**
```bash
# Check robots.txt and sitemap
curl http://target.com/robots.txt
curl http://target.com/sitemap.xml

# View page source for comments
view-source:http://target.com

# Directory enumeration
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt

# SQL injection quick test
' OR '1'='1' --
' OR 1=1--
admin' --

# XSS quick test
<script>alert(1)</script>
<img src=x onerror=alert(1)>
```

### Cryptography

**Common Challenges:**
```
- Classical ciphers (Caesar, Vigenère, substitution)
- RSA attacks (small e, weak primes)
- Hash cracking
- Block cipher modes
- Encoding confusion (base64, hex, binary)
```

**Approach:**
```bash
# Identify encoding/encryption
- Look for patterns
- Check entropy
- Try common encodings

# Classical ciphers
- Frequency analysis
- Try online cipher identifiers
- Use dcode.fr for quick decryption

# Hash identification
hashid hash_string
hash-identifier

# Hash cracking
hashcat -m 0 -a 0 hash.txt wordlist.txt  # MD5
john --wordlist=rockyou.txt hash.txt
```

### Binary Exploitation (Pwn)

**Common Techniques:**
```
- Buffer overflow
- Format string vulnerabilities
- Return-oriented programming (ROP)
- Use-after-free
- Integer overflow
- Shellcode injection
```

**Basic Approach:**
```python
# Using pwntools
from pwn import *

# Connect to service
p = remote('target.com', 1337)
# or for local testing
# p = process('./vulnerable_binary')

# Send payload
payload = b'A' * 64  # Overflow
payload += p64(0xdeadbeef)  # Overwrite return address

p.sendline(payload)
p.interactive()
```

**Common Protections to Check:**
```bash
# Check binary protections
checksec ./binary

# Look for:
# - NX (No eXecute) - Can't execute on stack
# - PIE (Position Independent Executable) - ASLR
# - Stack Canary - Buffer overflow protection
# - RELRO - GOT overwrite protection
```

### Reverse Engineering

**Approach:**
```
1. Run strings on binary
   strings binary | grep -i flag
   
2. Check for obfuscation
   - Packed/encrypted?
   - Use UPX, detect-it-easy
   
3. Static analysis
   - Ghidra/IDA
   - Understand program flow
   
4. Dynamic analysis
   - Run in debugger
   - Set breakpoints
   - Trace execution
```

**Useful Commands:**
```bash
# Extract strings
strings binary
strings -e l binary  # Little endian unicode

# File info
file binary
checksec binary

# Disassemble
objdump -d binary
radare2 -A binary

# Debug
gdb binary
# In GDB:
# b main
# r
# disas main
```

### Forensics

**Common Tasks:**
```
- Memory dumps analysis
- Disk image examination
- Network packet analysis
- File carving and recovery
- Metadata extraction
- Steganography detection
```

**Quick Checks:**
```bash
# File type and metadata
file evidence.img
exiftool image.jpg

# Extract embedded files
binwalk -e firmware.bin
foremost -i disk.img

# Memory analysis with Volatility
volatility -f memory.dump imageinfo
volatility -f memory.dump --profile=Win7SP1x64 pslist

# Network analysis
wireshark capture.pcap
tshark -r capture.pcap -Y "http"

# String search
strings -a file | grep -i "flag"
```

### OSINT (Open Source Intelligence)

**Techniques:**
```
- Google dorking
- Social media investigation
- DNS/WHOIS lookups
- Reverse image search
- Metadata analysis
- Public records search
```

**Tools:**
```bash
# Google dorking
site:target.com filetype:pdf
inurl:admin
intitle:"index of"

# Domain information
whois target.com
dig target.com ANY
nslookup target.com

# Social media tools
sherlock username
theHarvester -d target.com -b all

# Reverse image search
- Google Images
- TinEye
- Yandex Images
```

---

## 🏆 CTF Strategies and Tips

### General Strategy

**1. Start with Easy Challenges**
```
- Read all challenges first
- Start with low-point challenges
- Build momentum with quick solves
- Save hard challenges for later
```

**2. Categorize by Strength**
```
- Focus on categories you're good at
- Don't waste time on weaknesses early
- Collaborate with teammates
```

**3. Read Challenge Descriptions Carefully**
```
- Look for hints in description
- Note any provided files/links
- Understand what's being asked
```

**4. Keep Notes**
```
- Document your approach
- Save commands that work
- Track dead ends to avoid repeating
```

### Time Management

**Don't Get Stuck:**
```
- Set a time limit per challenge (e.g., 30 min)
- Move on if not progressing
- Come back with fresh perspective
- Ask teammates for help
```

**Low-Hanging Fruit:**
```
- Check common locations for flags
- Try obvious passwords
- Look for comments in source code
- Check for backup files (.bak, .old, ~)
```

### Common Flag Formats

**Be Aware of Flag Format:**
```
- flag{...}
- CTF{...}
- FLAG{...}
- Team_name{...}
- Custom format mentioned in rules
```

**Case Sensitivity:**
```
- Flags are usually case-sensitive
- Some CTFs wrap found data: flag{data}
- May need to format before submission
```

---

## 📚 Practice Platforms

### Beginner-Friendly
```
TryHackMe         - Guided learning with hints
PicoCTF           - Educational CTF by CMU
OverTheWire       - Wargames for beginners
HackThisSite      - Web-based challenges
```

### Intermediate
```
HackTheBox        - Boot2root machines
Root-Me           - Various challenges
WeChall           - Challenge aggregator
247CTF            - Always-on CTF
```

### Advanced
```
pwnable.kr        - Binary exploitation
pwnable.tw        - Advanced pwn
Microcorruption   - Embedded systems
CryptoHack        - Cryptography focused
```

### Live CTF Competitions
```
CTFtime.org       - Calendar of upcoming CTFs
DEFCON CTF        - Most prestigious
Google CTF        - By Google
Plaid CTF         - By PPP
```

---

## 🎓 Learning Resources

### Websites
```
- CTFtime.org - CTF calendar and team rankings
- CTF Field Guide - Beginner strategies
- LiveOverflow - YouTube tutorials
- IppSec - HackTheBox walkthroughs
```

### Books
```
- The Hacker Playbook 3
- Hacking: The Art of Exploitation
- The Web Application Hacker's Handbook
- Practical Malware Analysis
```

### Courses
```
- PWN College (pwn.college)
- Nightmare (guyinatuxedo.github.io)
- Modern Binary Exploitation (RPISEC)
```

---

## 🤝 Team Play

### Building a Team
```
- Mix of skills (web, crypto, pwn, RE, forensics)
- Good communication
- Available during competition times
- Practice together before competitions
```

### Communication
```
- Use Discord/Slack for real-time chat
- Share findings immediately
- Document solved challenges
- Help teammates when stuck
```

### Challenge Distribution
```
- Assign challenges by expertise
- Don't duplicate effort
- Rotate if someone is stuck
- Share successful techniques
```

---

## 💡 Common Mistakes to Avoid

```
❌ Overthinking simple challenges
❌ Not reading challenge descriptions fully
❌ Giving up too quickly on hard challenges
❌ Not taking breaks during long CTFs
❌ Poor time management
❌ Not using Google effectively
❌ Forgetting to submit flags
❌ Not learning from write-ups after CTF
```

---

## 📝 Write-ups

**After the CTF:**
```
1. Read write-ups from top teams
2. Learn new techniques
3. Understand better approaches
4. Document your own solutions
5. Share knowledge with community
```

**Resources for Write-ups:**
```
- CTFtime.org write-ups section
- Team blogs
- GitHub repositories
- Medium articles
```

---

## 🎯 CTF Checklist

### Before Competition
- [ ] Register team on CTFtime
- [ ] Set up communication channels
- [ ] Prepare tools and VMs
- [ ] Check competition rules and format
- [ ] Plan schedule for team availability

### During Competition
- [ ] Read all challenge descriptions
- [ ] Start with easy challenges
- [ ] Document progress
- [ ] Take breaks
- [ ] Communicate with team
- [ ] Don't forget to submit flags!

### After Competition
- [ ] Read write-ups
- [ ] Document learnings
- [ ] Share solutions
- [ ] Practice new techniques
- [ ] Plan for next CTF

---

*Play CTFs, sharpen skills, join the community. Every flag is a lesson learned.*
