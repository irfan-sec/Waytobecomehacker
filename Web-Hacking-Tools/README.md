# 🌐 Web Hacking Tools - Complete Arsenal

> Essential tools for web application security testing and penetration testing

---

## 📋 Overview

Web application security testing requires a diverse toolkit to identify vulnerabilities across different attack vectors. This section covers the most important tools used by penetration testers and security researchers for assessing web application security.

---

## 🎯 Tool Categories

### 🔍 **Reconnaissance & Discovery**
Tools for gathering information about web applications and discovering hidden content.

| Tool | Purpose | Skill Level |
|------|---------|-------------|
| [Gobuster](./Gobuster.md) | Directory/file/subdomain discovery | Beginner |
| [ffuf](./ffuf.md) | Fast web fuzzer for discovery | Intermediate |
| [Dirsearch](./Dirsearch.md) | Advanced directory discovery | Beginner |

### 🛡️ **Vulnerability Scanning**
Automated tools for identifying common web application vulnerabilities.

| Tool | Purpose | Skill Level |
|------|---------|-------------|
| [OWASP ZAP](./OWASP-ZAP.md) | Comprehensive web app security scanner | Beginner |
| [Nikto](./Nikto.md) | Web server vulnerability scanner | Beginner |
| [Nuclei](./Nuclei.md) | Fast vulnerability scanner with templates | Intermediate |

### 🔓 **Exploitation & Testing**
Tools for manually testing and exploiting discovered vulnerabilities.

| Tool | Purpose | Skill Level |
|------|---------|-------------|
| [Burp Suite](./BurpSuite.md) | Interactive web application security testing | Intermediate |
| [SQLMap](./SQLMap.md) | Automated SQL injection testing | Intermediate |
| [XSStrike](./XSStrike.md) | Advanced XSS detection and exploitation | Advanced |

### 💥 **Exploitation Frameworks**
Comprehensive frameworks for penetration testing and exploitation.

| Tool | Purpose | Skill Level |
|------|---------|-------------|
| [Metasploit](./Metasploit-Notes.md) | Complete penetration testing framework | Advanced |
| [BeEF](./BeEF.md) | Browser exploitation framework | Advanced |

### 🔐 **Authentication Testing**
Tools for testing authentication mechanisms and password security.

| Tool | Purpose | Skill Level |
|------|---------|-------------|
| [Hydra](./Hydra.md) | Network logon cracker | Intermediate |
| [Medusa](./Medusa.md) | Parallel password cracker | Intermediate |
| [Patator](./Patator.md) | Multi-purpose brute forcer | Advanced |

---

## 🚀 Getting Started

### For Beginners:
1. Start with **OWASP ZAP** for automated scanning
2. Learn **Gobuster** for directory discovery
3. Practice with **Nikto** for basic vulnerability scanning
4. Move to **Burp Suite** for manual testing

### For Intermediate Users:
1. Master **Burp Suite** professional features
2. Learn **SQLMap** for database testing
3. Practice with **Hydra** for authentication testing
4. Explore **ffuf** for advanced fuzzing

### For Advanced Users:
1. Deep dive into **Metasploit** framework
2. Learn **BeEF** for browser exploitation
3. Master **XSStrike** for XSS exploitation
4. Develop custom scripts and tools

---

## 🛠️ Essential Setup

### Kali Linux Installation
Most tools come pre-installed on Kali Linux:
```bash
# Update package list
sudo apt update && sudo apt upgrade -y

# Install additional tools if needed
sudo apt install gobuster hydra sqlmap nikto -y
```

### Manual Installation
For tools not in repositories:
```bash
# ffuf
go install github.com/ffuf/ffuf@latest

# Nuclei
go install -v github.com/projectdiscovery/nuclei/v2/cmd/nuclei@latest
```

---

## 📚 Learning Path

### Phase 1: Foundation (Weeks 1-2)
- [ ] Learn web application basics (HTTP, HTML, JavaScript)
- [ ] Set up testing environment (Kali Linux, DVWA)
- [ ] Practice with OWASP ZAP automated scans
- [ ] Learn Gobuster for directory discovery

### Phase 2: Manual Testing (Weeks 3-6)
- [ ] Master Burp Suite proxy and repeater
- [ ] Learn manual vulnerability testing techniques
- [ ] Practice with SQLMap for database attacks
- [ ] Understand OWASP Top 10 vulnerabilities

### Phase 3: Advanced Techniques (Weeks 7-12)
- [ ] Advanced Burp Suite features (Intruder, Extensions)
- [ ] Learn Metasploit for exploitation
- [ ] Practice with Hydra for brute forcing
- [ ] Develop custom testing scripts

### Phase 4: Specialization (Months 4-6)
- [ ] Choose specialization (API testing, mobile, etc.)
- [ ] Learn advanced exploitation frameworks
- [ ] Practice on real-world applications (with permission)
- [ ] Contribute to bug bounty programs

---

## 🎓 Recommended Learning Resources

### Hands-On Platforms
- **[TryHackMe](https://tryhackme.com/)** - Guided web hacking rooms
- **[PortSwigger Academy](https://portswigger.net/web-security)** - Free web security training
- **[OWASP WebGoat](https://owasp.org/www-project-webgoat/)** - Intentionally vulnerable application
- **[Damn Vulnerable Web Application (DVWA)](http://www.dvwa.co.uk/)** - Practice environment

### Books & Documentation
- **OWASP Testing Guide** - Comprehensive web app testing methodology
- **The Web Application Hacker's Handbook** - Classic reference
- **Real-World Bug Hunting** - Modern web vulnerabilities

### Video Training
- **Bugcrowd University** - Free web security courses
- **PentesterLab** - Hands-on vulnerability exercises
- **Cybrary** - Free cybersecurity training

---

## ⚖️ Legal and Ethical Guidelines

### ✅ DO:
- Test only on applications you own or have explicit permission to test
- Use responsible disclosure for vulnerabilities found
- Practice on dedicated vulnerable applications and labs
- Follow bug bounty program rules and scope

### ❌ DON'T:
- Test on applications without permission
- Cause damage or disruption to services
- Access or modify data that doesn't belong to you
- Ignore responsible disclosure practices

---

## 🤝 Contributing

Want to add a new tool guide or improve existing content?

1. **Fork** this repository
2. **Create** a new tool guide following the existing format
3. **Update** this README.md to include your tool
4. **Submit** a pull request

### Tool Guide Format
Each tool guide should include:
- Overview and purpose
- Installation instructions
- Basic usage examples
- Advanced techniques
- Real-world scenarios
- Learning resources

---

## 📬 Community & Support

- **Questions?** Open an issue in this repository
- **Discussions** Join cybersecurity communities on Discord/Reddit
- **Bug Bounty** Connect with researchers on platforms like HackerOne

---

<div class="notice--warning">
  <h4>🚨 Important Reminder</h4>
  <p>These tools are extremely powerful and should only be used for <strong>legitimate security testing</strong> with proper authorization. Unauthorized use is illegal and unethical. Always practice responsible disclosure and follow applicable laws and regulations.</p>
</div>

---

*Made with ❤️ for the cybersecurity community. Practice ethical hacking and help make the web safer for everyone.*