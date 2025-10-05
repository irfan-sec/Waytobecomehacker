---
title: "Capture The Flag (CTF) Guide"
permalink: /CTF/
layout: single
author_profile: true
toc: true
toc_sticky: true
---

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
sqlmap          # SQL injection
gobuster        # Directory brute forcing
nikto           # Web scanner
```

### Cryptography
```bash
# Crypto tools
openssl         # Encryption/decryption
john            # Password cracking
hashcat         # Hash cracking
CyberChef       # Data encoding/decoding
```

### Binary Exploitation
```bash
# Binary analysis
gdb             # Debugger
pwntools        # Exploit development
checksec        # Binary security checker
ROPgadget       # ROP chain builder
```

### Forensics
```bash
# File analysis
binwalk         # Firmware analysis
foremost        # File carving
volatility      # Memory forensics
autopsy         # Digital forensics
strings         # Extract readable strings
exiftool        # Metadata analysis
```

### OSINT
```bash
# Information gathering
theHarvester    # Email/subdomain gathering
recon-ng        # Web reconnaissance
Maltego         # Link analysis
SpiderFoot      # OSINT automation
```

---

## 🎯 Getting Started with CTFs

### For Complete Beginners
1. **Start with beginner-friendly platforms:**
   - [PicoCTF](https://picoctf.org/) - Educational CTF for beginners
   - [TryHackMe](https://tryhackme.com/) - Guided learning paths
   - [OverTheWire](https://overthewire.org/) - Wargames for learning

2. **Learn the basics:**
   - Linux command line
   - Basic networking
   - Programming (Python recommended)
   - Web technologies (HTML, JavaScript, HTTP)

3. **Practice daily:**
   - Solve at least one challenge per day
   - Read writeups from others
   - Document your own solutions

### Popular CTF Platforms

| Platform | Difficulty | Style | Best For |
|----------|-----------|-------|----------|
| [PicoCTF](https://picoctf.org/) | Beginner | Educational | Students |
| [TryHackMe](https://tryhackme.com/) | Beginner-Intermediate | Guided | Learning |
| [HackTheBox](https://hackthebox.eu/) | Intermediate-Advanced | Independent | Practice |
| [Root-Me](https://www.root-me.org/) | All levels | Jeopardy | Skills building |
| [CTFtime](https://ctftime.org/) | All levels | Competition listing | Competition |
| [CryptoHack](https://cryptohack.org/) | All levels | Crypto-focused | Cryptography |

---

## 📚 Challenge Categories in Detail

### Web Exploitation
**Common Techniques:**
- SQL Injection
- Cross-Site Scripting (XSS)
- Command Injection
- File Upload vulnerabilities
- Server-Side Request Forgery (SSRF)
- Directory Traversal

**Learning Resources:**
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [OWASP Top 10](../OWASP/)
- [Web Hacking Tools](../Web-Hacking-Tools/)

### Cryptography
**Common Topics:**
- Classical ciphers (Caesar, Vigenere)
- Modern encryption (AES, RSA)
- Hash functions
- Block cipher modes
- Public key cryptography
- Weak implementations

**Learning Resources:**
- [CryptoHack](https://cryptohack.org/)
- [Cryptopals Challenges](https://cryptopals.com/)
- [Cryptography Basics](../Cryptography/)

### Binary Exploitation (Pwn)
**Common Techniques:**
- Buffer overflow
- Return-oriented programming (ROP)
- Format string vulnerabilities
- Heap exploitation
- Use-after-free

**Prerequisites:**
- C programming
- Assembly language
- Computer architecture
- Operating systems

### Reverse Engineering
**Common Tasks:**
- Static analysis
- Dynamic analysis
- Code deobfuscation
- Anti-debugging bypasses
- Malware analysis

**Tools:**
- Ghidra, IDA Pro, Binary Ninja
- x64dbg, OllyDbg
- dnSpy (for .NET)
- JD-GUI (for Java)

### Forensics
**Common Tasks:**
- File recovery
- Memory dump analysis
- Network packet analysis
- Steganography
- Log analysis
- Disk imaging

**Learning Path:**
- Start with file formats
- Learn steganography basics
- Practice with Wireshark
- Study memory forensics

### OSINT (Open Source Intelligence)
**Common Tasks:**
- Finding information about people/organizations
- Social media investigation
- Domain and IP research
- Metadata extraction
- Google dorking

**Framework:**
1. Passive information gathering
2. Active reconnaissance
3. Data aggregation
4. Analysis and reporting

---

## 🏆 CTF Strategy and Tips

### Before the Competition
```
✅ Form a diverse team (different specializations)
✅ Set up your tools and environment
✅ Review common techniques
✅ Prepare your note-taking system
✅ Get familiar with the platform
✅ Ensure stable internet connection
```

### During the Competition
```
✅ Read all challenges first
✅ Start with categories you're strongest in
✅ Work on low-hanging fruit (easy points)
✅ Don't get stuck on one challenge
✅ Communicate with your team
✅ Take breaks
✅ Document your findings
✅ Submit flags immediately when found
```

### After the Competition
```
✅ Read writeups from top teams
✅ Try challenges you didn't solve
✅ Document what you learned
✅ Share your own writeups
✅ Identify areas for improvement
✅ Thank organizers and other teams
```

---

## 💡 Pro Tips

### Improve Your Skills
1. **Specialize but don't limit yourself** - Be strong in 2-3 categories but try everything
2. **Learn from writeups** - Study solutions from top teams
3. **Practice regularly** - Consistency is key
4. **Join a team** - Learn from teammates
5. **Automate repetitive tasks** - Build your own tools

### Common Mistakes to Avoid
```
❌ Spending too long on one challenge
❌ Not reading challenge descriptions carefully
❌ Ignoring hints and updated information
❌ Not documenting your approach
❌ Working alone when stuck
❌ Giving up too easily
❌ Not managing time effectively
```

### Tools Setup
```bash
# Create a CTF toolkit directory
mkdir -p ~/ctf-tools && cd ~/ctf-tools

# Essential repositories
git clone https://github.com/Gallopsled/pwntools
git clone https://github.com/radareorg/radare2
git clone https://github.com/pwndbg/pwndbg

# Install common tools
sudo apt install -y gdb python3-pip binwalk foremost
pip3 install pwntools
```

---

## 📖 Learning Path

### Month 1: Foundations
- [ ] Master Linux command line
- [ ] Learn Python basics
- [ ] Complete TryHackMe beginner rooms
- [ ] Solve PicoCTF challenges
- [ ] Read CTF writeups

### Month 2-3: Specialization
- [ ] Choose 2-3 focus categories
- [ ] Complete category-specific challenges
- [ ] Learn relevant tools deeply
- [ ] Start competing in online CTFs
- [ ] Write your own writeups

### Month 4-6: Competition
- [ ] Join a CTF team
- [ ] Compete regularly (1-2 CTFs per month)
- [ ] Build your own tools/scripts
- [ ] Contribute to open-source security tools
- [ ] Help mentor beginners

---

## 🔗 Recommended Resources

### Writeup Collections
- [CTFtime Writeups](https://ctftime.org/writeups)
- [GitHub CTF Writeups](https://github.com/topics/ctf-writeups)
- Individual team blogs

### Practice Platforms
- **[PicoCTF](https://picoctf.org/)** - Best for beginners
- **[TryHackMe](https://tryhackme.com/)** - Guided learning
- **[HackTheBox](https://www.hackthebox.eu/)** - Real-world systems
- **[Root-Me](https://www.root-me.org/)** - Wide variety of challenges

### Communities
- Reddit: r/securityCTF, r/netsec
- Discord: CTF servers, security communities
- IRC: Freenode security channels

---

## 🎓 Additional Resources

For more detailed information, check out:
- **[CTF Guide (Full)](./CTF-Guide/)** - Complete methodology and techniques
- **[Web Security](../Web-Security/)** - Web application security
- **[Networking](../Networking/)** - Network fundamentals
- **[Cryptography](../Cryptography/)** - Crypto concepts

---

<div class="notice--info">
  <h4>🏁 Ready to Start?</h4>
  <p>CTFs are about learning, not just winning. Every challenge you solve makes you a better security professional. Start with easy challenges, be persistent, and don't be afraid to ask for help. Good luck!</p>
</div>

---

*"The expert in anything was once a beginner. Keep solving, keep learning!"* 🚩
