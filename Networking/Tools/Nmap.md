---
---
# Nmap Guide

A comprehensive guide to **Nmap**, covering port scanning, scan types, the Nmap Scripting Engine (NSE), and firewall evasion techniques for penetration testing and network reconnaissance.

---

## 📌 Table of Contents

1. Introduction
2. Nmap Switches
3. Scan Types Overview
4. TCP Connect Scans (-sT)
5. SYN Scans (-sS)
6. UDP Scans (-sU)
7. NULL, FIN, and Xmas Scans
8. Ping Sweep / ICMP Scans (-sn)
9. Nmap Scripting Engine (NSE)
10. Using NSE Scripts & Arguments
11. Finding & Installing NSE Scripts
12. Firewall Evasion & Stealth Scanning

---

## 1. Introduction

When it comes to penetration testing, **knowledge is power**. Proper **enumeration** is crucial before any exploitation attempt.

* **Port scanning** is the first step to map out a network:

  * Identify active services and open ports
  * Detect firewalls and filtered ports
  * Prepare for service-specific enumeration

**Key points:**

* Every computer has **65,535 ports**
* Common ports: **80 (HTTP)**, **443 (HTTPS)**, **139 (NetBIOS)**, **445 (SMB)**
* Enumeration is essential before any attack

Tool of choice: **Nmap**, the industry standard for port scanning and service discovery.

---

## 2. Nmap Switches

Nmap is primarily run from the terminal:

`nmap [options] [target]`

* **Help menu:** `nmap -h`
* **Manual page:** `man nmap`

**Examples of useful switches:**

* `-p [ports]` → Specify ports
* `-sV` → Detect service versions
* `-O` → OS detection
* `-A` → Aggressive scan (OS + version + script scan + traceroute)

---

## 3. Scan Types Overview

Nmap supports multiple scan types:

1. **TCP Connect Scans (`-sT`)**
2. **SYN "Half-open" Scans (`-sS`)**
3. **UDP Scans (`-sU`)**

Less common scans:

* **NULL Scans (`-sN`)**
* **FIN Scans (`-sF`)**
* **Xmas Scans (`-sX`)**

---

## 4. TCP Connect Scans (-sT)

Performs a **full TCP three-way handshake** with each port:

1. **SYN** → **SYN/ACK** → **ACK**
2. If port is closed → Server replies with **RST**
3. If port is filtered → No response

**Example:**
`sudo nmap -sT [target]`

Pros:

* Simple, reliable

Cons:

* Easily detectable in logs

---

## 5. SYN Scans (-sS)

Also known as **Half-open / Stealth scans**:

* Sends a **SYN** and waits for **SYN/ACK**
* Sends **RST** immediately instead of completing handshake

**Example:**
`sudo nmap -sS [target]`

Advantages:

* Stealthier (less likely to be logged)
* Faster than TCP Connect scans

---

## 6. UDP Scans (-sU)

* UDP is **stateless** → No handshake
* Open ports often **don’t respond** → Marked as `open|filtered`
* Closed ports send **ICMP Port Unreachable**

**Example:**
`sudo nmap -sU --top-ports 20 [target]`

⚠ **Note:** UDP scans are **very slow** (\~20 mins for 1000 ports).

---

## 7. NULL, FIN, and Xmas Scans

* **NULL Scan (`-sN`)** → Sends packet with **no flags**
* **FIN Scan (`-sF`)** → Sends packet with **FIN flag only**
* **Xmas Scan (`-sX`)** → Sends packet with **PSH, URG, FIN flags**

Behavior:

* **Closed port** → Responds with **RST**
* **Open port or filtered** → No response (`open|filtered`)

**Examples:**
`sudo nmap -sN [target]`
`sudo nmap -sF [target]`
`sudo nmap -sX [target]`

---

## 8. Ping Sweep / ICMP Scans (-sn)

Use to discover **active hosts** on a network:

`nmap -sn 192.168.0.1-254`
`nmap -sn 192.168.0.0/24`

* `-sn` → Skip port scanning, only host discovery
* Sends **ICMP Echo**, **TCP SYN (443)**, and **TCP ACK (80)**

---

## 9. Nmap Scripting Engine (NSE)

The **NSE** uses Lua scripts to extend Nmap’s functionality.

**Categories:**

* `safe` → Non-intrusive
* `intrusive` → May affect target
* `vuln` → Vulnerability scanning
* `exploit` → Exploit vulnerabilities
* `auth` → Authentication bypass
* `brute` → Brute-force credentials
* `discovery` → Additional information gathering

---

## 10. Using NSE Scripts & Arguments

Run scripts by category:
`nmap --script=vuln [target]`

Run multiple scripts:
`nmap --script=smb-enum-users,smb-enum-shares [target]`

Use script arguments:
`nmap -p 80 --script http-put --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php'`

Check script help:
`nmap --script-help [script-name]`

---

## 11. Finding & Installing NSE Scripts

Scripts are stored in:
`/usr/share/nmap/scripts`

Search locally:
`grep "ftp" /usr/share/nmap/scripts/script.db`
`ls -l /usr/share/nmap/scripts/*ftp*`

Install new scripts:

```
sudo wget -O /usr/share/nmap/scripts/[script-name].nse https://svn.nmap.org/nmap/scripts/[script-name].nse
sudo nmap --script-updatedb
```

---

## 12. Firewall Evasion & Stealth Scanning

**Bypass ICMP-blocking firewalls:**
`nmap -Pn [target]`

Other useful switches:

* `-f` → Fragment packets
* `--mtu [size]` → Custom packet size (multiple of 8)
* `--scan-delay [ms]` → Delay between packets (evade IDS/timing triggers)
* `--badsum` → Send packets with invalid checksums (test firewalls)

---

## ✅ Summary

* Start with **host discovery** and **port scanning**
* Use **appropriate scan types** for stealth and speed
* Leverage **NSE scripts** for vulnerability detection
* Apply **firewall evasion techniques** when necessary

Nmap is a **powerful reconnaissance tool** when used properly, forming the foundation for any penetration test or security audit.
