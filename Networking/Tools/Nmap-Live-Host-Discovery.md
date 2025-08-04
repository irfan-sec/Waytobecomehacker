
# Nmap Live Host Discovery

This guide explains how to efficiently discover live hosts on a network using **Nmap**, along with complementary tools like `arp-scan` and `masscan`.  
It covers host discovery techniques, command-line options, and best practices for active reconnaissance.

---

## 📌 Introduction to Nmap

When targeting a network, one of the first tasks is to determine:

1. **Which systems are up?**  
2. **What services are running on these systems?**  

Nmap (Network Mapper), created by Gordon Lyon (Fyodor) in 1997, is an **industry-standard** open-source tool for:

- Mapping networks  
- Identifying live hosts  
- Discovering running services  
- Performing advanced scripting via NSE (Nmap Scripting Engine)  

In this guide, we focus on **host discovery**, which answers the first question: *Which systems are up?*

---

## 🌐 Network Segments & Subnets

Before scanning, it’s important to understand how networks are structured:

- **Network Segment:** A group of computers sharing the same physical medium (Ethernet switch or Wi-Fi AP).  
- **Subnet (IP subnetwork):** One or more network segments using the same router and subnet mask.

### Common Subnet Examples:

- `/16` → `255.255.0.0` → ~65,000 hosts  
- `/24` → `255.255.255.0` → ~250 hosts  

ARP queries work **only within the same subnet**, because ARP is a **link-layer protocol** and does not cross routers.

---

## 🎯 Target Specification in Nmap

You can specify targets in several ways:

- **List:**  
  ```bash
  nmap MACHINE_IP scanme.nmap.org example.com
````

(Scans 3 IP addresses)

* **Range:**

  ```bash
  nmap 10.11.12.15-20
  ```

  (Scans 6 IPs: `.15` → `.20`)

* **Subnet:**

  ```bash
  nmap MACHINE_IP/30
  ```

  (Scans 4 IPs)

* **From File:**

  ```bash
  nmap -iL list_of_hosts.txt
  ```

Preview the hosts Nmap will scan without actually scanning:

```bash
nmap -sL TARGETS
```

Skip DNS lookups during this step:

```bash
nmap -sL -n TARGETS
```

---

## 📡 TCP/IP Layers & Protocols in Scanning

Nmap leverages multiple protocols for host discovery:

![TCP/IP Layers](2b66cb6c-cfd0-4af0-89a7-9fc43b63cd1b.png)

* **ARP** (Link Layer) → Discover hosts in the same subnet
* **ICMP** (Network Layer) → Ping and other echo requests
* **TCP** (Transport Layer) → SYN or ACK ping
* **UDP** (Transport Layer) → Detects live hosts via ICMP unreachable responses

---

## 🔍 Host Discovery Techniques

### 1️⃣ ARP Scan

* **Usage:** Only works on the same subnet
* **Command:**

  ```bash
  sudo nmap -PR -sn MACHINE_IP/24
  ```
* Nmap sends ARP requests and listens for replies.
* Alternative tool: `arp-scan`

  ```bash
  sudo arp-scan --localnet
  sudo arp-scan -I eth0 -l
  ```

---

### 2️⃣ ICMP Echo Scan

* **Simple ping method** (ICMP Type 8 → Echo Request / Type 0 → Echo Reply)
* **Command:**

  ```bash
  sudo nmap -PE -sn MACHINE_IP/24
  ```
* ⚠️ Often blocked by firewalls or modern Windows systems
* ARP query precedes ICMP if on the same subnet

---

### 3️⃣ TCP SYN Ping

* **Command:**

  ```bash
  sudo nmap -PS22,80,443 -sn MACHINE_IP/30
  ```
* Sends SYN packets and checks for any response (SYN/ACK → Host Up)
* Privileged users can send SYN without completing the handshake

---

### 4️⃣ TCP ACK Ping

* **Command:**

  ```bash
  sudo nmap -PA80,443,8080 -sn MACHINE_IP/30
  ```
* Sends an ACK packet → Host responds with **RST** if up
* Requires privileged user to send custom TCP packets

---

### 5️⃣ UDP Ping

* **Command:**

  ```bash
  sudo nmap -PU53,161,162 -sn MACHINE_IP/30
  ```
* Open UDP ports often give no reply
* Closed ports trigger **ICMP Port Unreachable**, indicating host is online

---

### 6️⃣ Masscan Overview

* **High-speed scanner** for discovering hosts
* Similar syntax to Nmap:

  ```bash
  masscan MACHINE_IP/24 -p443
  masscan MACHINE_IP/24 -p80,443
  masscan MACHINE_IP/24 -p22-25
  masscan MACHINE_IP/24 --top-ports 100
  ```

> ⚠️ Masscan is very aggressive and can overwhelm networks.

---

## 🌎 Reverse DNS Lookups in Nmap

* Nmap performs **reverse DNS lookups** on live hosts by default
* Useful for extracting hostnames, which may reveal system info

Options:

* Skip DNS lookup:

  ```bash
  nmap -n TARGETS
  ```
* Query DNS for all hosts (even offline):

  ```bash
  nmap -R TARGETS
  ```
* Use a specific DNS server:

  ```bash
  nmap --dns-servers 8.8.8.8 TARGETS
  ```

---

## 📖 Summary & Quick Reference

### Nmap Host Discovery Commands

| Scan Type           | Example Command                             |
| ------------------- | ------------------------------------------- |
| ARP Scan            | `sudo nmap -PR -sn MACHINE_IP/24`           |
| ICMP Echo Scan      | `sudo nmap -PE -sn MACHINE_IP/24`           |
| ICMP Timestamp Scan | `sudo nmap -PP -sn MACHINE_IP/24`           |
| ICMP Address Mask   | `sudo nmap -PM -sn MACHINE_IP/24`           |
| TCP SYN Ping        | `sudo nmap -PS22,80,443 -sn MACHINE_IP/30`  |
| TCP ACK Ping        | `sudo nmap -PA22,80,443 -sn MACHINE_IP/30`  |
| UDP Ping            | `sudo nmap -PU53,161,162 -sn MACHINE_IP/30` |

> **Tip:** Always add `-sn` for host discovery only. Without it, Nmap proceeds to port scan live hosts.

---

### Useful Options

| Option | Purpose                                 |
| ------ | --------------------------------------- |
| `-n`   | Disable DNS lookup                      |
| `-R`   | Reverse-DNS lookup for all hosts        |
| `-sn`  | Host discovery only, skip port scanning |

---

## ✅ Conclusion

By mastering ARP, ICMP, TCP, and UDP scans, you can reliably discover live hosts. Any valid response from a host indicates it is online.
Nmap, combined with tools like `arp-scan` and `masscan`, provides a powerful toolkit for active reconnaissance.

```
