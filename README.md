![Status](https://img.shields.io/badge/status-active-brightgreen)
![Built With](https://img.shields.io/badge/built%20with-Jekyll-blue)
![Theme](https://img.shields.io/badge/theme-Chirpy-purple)
![Hosted On](https://img.shields.io/badge/hosted%20on-GitHub%20Pages-lightgrey)

# 🧠 Way to become Hacker

> A practical guide to launching your cyber security career — from total beginner to job-ready roles.  
> Inspired by TryHackMe’s learning paths, this repo outlines how to specialize in different cyber security domains.

---
# Introduction
Cyber security careers are becoming more in demand and offer high salaries. There are many different jobs within the security industry, from offensive pentesting (hacking machines and reporting on vulnerabilities) to defensive security (defending against and investigating cyberattacks).

Why get a career in cyber:

 * High Pay - jobs in security have high starting salaries
 * Exciting - work can include legally hacking systems or defending against cyber attacks
 * Be in demand - there are over 3.5 million unfilled cyber positions
 * This room helps you break into cyber security by providing information about various cyber security roles; it also links to different learning paths that you can use to start building your cyber skills.

## 🔐 What is This Repo?

**`waytobecomehacker`** is a roadmap-style collection of cyber security career paths, with learning resources, responsibilities, skills, and recommended TryHackMe training for each role. Whether you're starting from scratch or pivoting from IT, this is your launchpad into ethical hacking and defensive security careers.

---

## 🛣️ Career Paths For Cyber Security

| Role | Description |
|------|-------------|
| [🛡️ Security Analyst](./SecurityAnalyst.md) | Monitor, analyze, and report on threats across systems and networks |
| [🔧 Security Engineer](./SecurityEngineer.md) | Build and maintain secure infrastructure, systems, and controls |
| [🚨 Incident Responder](./IncidentResponder.md) | Detect, contain, and recover from live cyberattacks |
| [🧪 Digital Forensics Examiner](./DigitalForensicsExaminer.md) | Investigate cyber incidents and gather digital evidence |
| [🧬 Malware Analyst](./MalwareAnalyst.md) | Reverse-engineer malicious software to understand and defend |
| [💥 Penetration Tester](./PenetrationTester.md) | Ethically hack systems to find and report security flaws |

---

## 🧭 Recommended Learning Platform

All paths are based on **[TryHackMe](https://tryhackme.com/)** — a hands-on cyber security platform that teaches practical skills using guided labs and virtual environments.

---

## 📁 Resources To Learning
[Networking](./Networking/)
Basic Web Tech

---

## 🛠️ Tools You Might Use Along the Way

- [Nmap](Networking/Tools/Nmap.md), [Burp Suite](Web-Hacking-Tools/BurpSuite.md), [Wireshark](Networking/Tools/Wireshark-Roadmap.md)
- [Metasploit](Web-Hacking-Tools/Metasploit-Notes.md), Hydra, Gobuster
- Ghidra, IDA Pro, Cuckoo Sandbox
- Autopsy, FTK Imager, Volatility
- Splunk, ELK Stack, SIEMs

---

## 💬 Why This Repo?

This project exists to:

✅ Help beginners choose a cyber security career path  
✅ Provide practical roadmaps with real-world tools  
✅ Centralize the best free training content  
✅ Make hacking education accessible to everyone  

---

## 🚀 My Learning Roadmap

- [x] Introduction to Cybersecurity
- [x] Networking Basics
- [ ] Linux Fundamentals
- [ ] Scripting with Bash and Python
- [ ] Web Application Security
- [ ] Vulnerability Scanning (e.g., Nmap, Nessus)
- [ ] Exploitation Basics (Metasploit, manual techniques)
- [ ] Capture The Flag (CTF) practice


---

## 🙋‍♂️ Who Is This For?

- Students and beginners in tech  
- IT professionals transitioning into security  
- Hobbyists and ethical hackers  
- Anyone curious about how to become a hacker — the right way

---

## 📬 Contributions

Want to add your own notes, tools, or tutorials? Feel free to fork this repo and submit a pull request. Contributions are welcome!

---

## 🧨 Disclaimer

This repo is for **educational** and **ethical** purposes only. Use your skills responsibly and legally.

---

Made with ❤️ using [TryHackMe](https://tryhackme.com/) paths and personal learning notes.

---

## 📝 Contributing to the Blog

This site is built with **Jekyll** using the **Chirpy** theme and hosted on **GitHub Pages**. Here's how to contribute content:

### Adding Blog Posts

1. **Create a new markdown file** in the `_posts/` directory
2. **Use the naming convention**: `YYYY-MM-DD-title-with-hyphens.md`
3. **Add front matter** at the top:

```yaml
---
title: "Your Post Title"
date: 2024-01-20 14:30:00 +0000
categories: [Category1, Category2]
tags: [tag1, tag2, tag3]
---
```

4. **Write your content** in Markdown below the front matter

### Available Categories
- `[Getting Started, Beginner]` - For newcomers to cybersecurity
- `[Tools, Professional Development]` - Tool tutorials and guides
- `[Home Lab, Practical Guide]` - Hands-on setup guides
- `[Career Advice, Industry]` - Professional development content

### Suggested Tags
Use relevant tags like: `cybersecurity`, `tools`, `nmap`, `burp-suite`, `kali-linux`, `career`, `beginner`, `homelab`, etc.

### Adding Career Path Pages

Career path pages go in the root directory with front matter:

```yaml
---
layout: page
title: "🔐 Your Role Career Path"
permalink: /YourRole/
---
```

### Local Development

To test changes locally:

```bash
# Install dependencies
bundle install

# Serve the site locally
bundle exec jekyll serve

# View at http://localhost:4000/Waytobecomehacker
```

### Guidelines

- **Write practical, actionable content**
- **Include code examples and commands when relevant**
- **Use clear headings and structure**
- **Add links to related content**
- **Keep cybersecurity ethics in mind**
