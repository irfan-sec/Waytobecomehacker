# 🐧 Linux Fundamentals for Cybersecurity

> Essential Linux knowledge for ethical hackers and security professionals

---

## 📋 Overview

Linux is the backbone of cybersecurity. Most penetration testing tools, servers, and security infrastructure run on Linux. Understanding Linux is crucial for anyone pursuing a career in cybersecurity, whether in offensive or defensive roles.

**Why Linux for Security?**
- 🔓 Open source and customizable
- 🛠️ Most security tools are Linux-based
- 🖥️ Majority of servers run Linux
- 🎯 Essential for penetration testing
- 🔒 Better security controls and permissions
- 💻 Command-line mastery is powerful

---

## 🎯 What You'll Learn

1. **Basic Commands** - Navigate and manipulate the file system
2. **File Permissions** - Understand and modify access controls
3. **User Management** - Create, modify, and manage users/groups
4. **Process Management** - Monitor and control running processes
5. **Network Commands** - Essential networking tools and utilities
6. **Text Processing** - Manipulate and analyze text files
7. **Shell Scripting** - Automate security tasks
8. **Package Management** - Install and manage software
9. **System Monitoring** - Track system resources and logs
10. **Security Hardening** - Secure your Linux systems

---

## 🚀 Getting Started

### Recommended Distributions
```bash
# For Penetration Testing
Kali Linux          # Most popular, comes with 600+ tools
ParrotOS           # Alternative to Kali, privacy-focused
BlackArch          # Arch-based, 2000+ tools

# For Learning
Ubuntu             # User-friendly, great for beginners
Debian             # Stable, secure
CentOS/Rocky       # Enterprise-focused
```

### Setting Up Practice Environment
```bash
# Option 1: VirtualBox/VMware
1. Download Kali Linux ISO
2. Create virtual machine
3. Install and configure

# Option 2: Docker
docker pull kalilinux/kali-rolling
docker run -it kalilinux/kali-rolling /bin/bash

# Option 3: WSL2 (Windows)
wsl --install -d kali-linux
```

---

## 📚 Essential Commands

### File System Navigation
```bash
# Print working directory
pwd

# List files and directories
ls                  # Basic listing
ls -la              # Detailed with hidden files
ls -lh              # Human-readable sizes
ls -R               # Recursive listing

# Change directory
cd /path/to/dir     # Absolute path
cd ..               # Parent directory
cd ~                # Home directory
cd -                # Previous directory

# Create directories
mkdir dirname
mkdir -p path/to/nested/dir

# Remove files and directories
rm file.txt
rm -r directory/    # Recursive
rm -rf directory/   # Force remove (careful!)

# Copy and move
cp source.txt dest.txt
cp -r source/ dest/
mv oldname.txt newname.txt
```

### File Viewing and Manipulation
```bash
# View file contents
cat file.txt        # Display entire file
less file.txt       # Paginated view (q to quit)
head file.txt       # First 10 lines
head -n 20 file.txt # First 20 lines
tail file.txt       # Last 10 lines
tail -f /var/log/syslog  # Follow log file

# Search within files
grep "pattern" file.txt
grep -r "pattern" directory/
grep -i "pattern" file.txt    # Case insensitive
grep -v "pattern" file.txt    # Invert match

# Find files
find /path -name "*.txt"
find /path -type f -name "*.log"
find /path -mtime -7          # Modified in last 7 days
find /path -size +100M        # Files larger than 100MB

# File information
file filename       # Determine file type
stat filename       # Detailed file statistics
wc file.txt         # Count lines, words, characters
```

### File Permissions
```bash
# Understanding permissions
# r = read (4), w = write (2), x = execute (1)
# Format: owner-group-others

# View permissions
ls -l

# Change permissions
chmod 755 file.sh       # rwxr-xr-x
chmod +x script.sh      # Add execute
chmod -w file.txt       # Remove write
chmod u+x,g-w file.sh   # User add execute, group remove write

# Change ownership
chown user:group file.txt
chown -R user:group directory/

# Special permissions
chmod u+s file          # SUID bit
chmod g+s directory     # SGID bit
chmod +t directory      # Sticky bit
```

---

## 👥 User and Group Management

### User Operations
```bash
# Add user
sudo useradd username
sudo useradd -m -s /bin/bash username  # With home directory and shell

# Set/change password
sudo passwd username

# Delete user
sudo userdel username
sudo userdel -r username  # Remove home directory too

# User information
id                      # Current user info
whoami                  # Current username
who                     # Logged in users
w                       # Detailed user info
last                    # Login history

# Switch user
su - username
sudo -u username command
```

### Group Operations
```bash
# Create group
sudo groupadd groupname

# Add user to group
sudo usermod -aG groupname username

# List user's groups
groups username

# Delete group
sudo groupdel groupname

# View all groups
cat /etc/group
```

---

## ⚙️ Process Management

### Viewing Processes
```bash
# List processes
ps                  # Current shell processes
ps aux              # All processes (detailed)
ps -ef              # Full format listing

# Interactive process viewer
top                 # Real-time process monitoring
htop                # Enhanced top (if installed)

# Process tree
pstree
ps auxf             # Process tree with ps

# Search for process
ps aux | grep process_name
pgrep process_name
pidof process_name
```

### Managing Processes
```bash
# Start process in background
command &

# Bring to foreground
fg

# Send to background
bg

# Kill processes
kill PID                # Graceful termination
kill -9 PID             # Force kill
killall process_name    # Kill by name
pkill process_name      # Kill by pattern

# Process priority
nice -n 10 command      # Start with priority
renice -n 5 -p PID      # Change priority

# Monitor resource usage
uptime                  # System load
free -h                 # Memory usage
df -h                   # Disk usage
du -sh directory/       # Directory size
```

---

## 🌐 Networking Commands

### Network Information
```bash
# IP configuration
ip addr show            # Show IP addresses
ip a                    # Short form
ifconfig                # Legacy command

# Routing
ip route show
route -n
netstat -rn

# Network statistics
netstat -tuln           # Listening ports
ss -tuln                # Modern alternative to netstat
ss -tunap               # All connections with process info

# DNS lookup
nslookup domain.com
dig domain.com
host domain.com

# Network connectivity
ping -c 4 8.8.8.8       # Test connectivity
traceroute google.com   # Trace network path
mtr google.com          # Combined ping and traceroute
```

### Network Tools
```bash
# Download files
wget https://example.com/file.txt
curl -O https://example.com/file.txt
curl -s https://api.example.com/data | jq

# Network scanning (covered in detail in Nmap section)
nmap -sn 192.168.1.0/24

# Port scanning
nc -zv host 1-1000      # Netcat port scan
telnet host port        # Test specific port

# Capture network traffic
tcpdump -i eth0
tcpdump -i eth0 port 80
```

---

## 📝 Text Processing

### Stream Editors
```bash
# sed - Stream editor
sed 's/old/new/' file.txt           # Replace first occurrence
sed 's/old/new/g' file.txt          # Replace all
sed -i 's/old/new/g' file.txt       # In-place edit
sed -n '10,20p' file.txt            # Print lines 10-20

# awk - Pattern scanning
awk '{print $1}' file.txt           # Print first column
awk -F: '{print $1}' /etc/passwd    # Custom delimiter
awk '$3 > 100' file.txt             # Conditional printing

# cut - Cut columns
cut -d: -f1 /etc/passwd             # First field
cut -c1-10 file.txt                 # Characters 1-10

# sort and uniq
sort file.txt
sort -r file.txt                    # Reverse
sort -n file.txt                    # Numeric sort
sort file.txt | uniq                # Remove duplicates
sort file.txt | uniq -c             # Count occurrences
```

### Text Analysis
```bash
# Word count
wc -l file.txt          # Lines
wc -w file.txt          # Words
wc -c file.txt          # Characters

# Compare files
diff file1.txt file2.txt
cmp file1.txt file2.txt

# String operations
tr 'a-z' 'A-Z' < file.txt           # Convert to uppercase
rev file.txt                         # Reverse lines
tac file.txt                         # Reverse file (last line first)
```

---

## 📦 Package Management

### Debian/Ubuntu (APT)
```bash
# Update package list
sudo apt update

# Upgrade packages
sudo apt upgrade
sudo apt full-upgrade

# Install package
sudo apt install package-name

# Remove package
sudo apt remove package-name
sudo apt purge package-name     # Remove with configs

# Search packages
apt search keyword
apt list --installed

# Package information
apt show package-name
```

### Red Hat/CentOS (YUM/DNF)
```bash
# Update system
sudo yum update
sudo dnf update

# Install package
sudo yum install package-name
sudo dnf install package-name

# Remove package
sudo yum remove package-name

# Search
yum search keyword
```

### Arch (Pacman)
```bash
# Update system
sudo pacman -Syu

# Install package
sudo pacman -S package-name

# Remove package
sudo pacman -R package-name

# Search
pacman -Ss keyword
```

---

## 🔍 System Monitoring

### Log Files
```bash
# Important log locations
/var/log/syslog         # System logs
/var/log/auth.log       # Authentication logs
/var/log/apache2/       # Apache logs
/var/log/nginx/         # Nginx logs

# View logs
sudo tail -f /var/log/syslog
sudo journalctl -f              # Systemd logs
sudo journalctl -u ssh          # Service-specific logs
sudo journalctl --since today   # Today's logs

# Search logs
grep "error" /var/log/syslog
zgrep "pattern" /var/log/syslog.*.gz  # Search compressed logs
```

### System Information
```bash
# System info
uname -a                # Kernel info
hostnamectl             # System hostname info
lsb_release -a          # Distribution info

# Hardware info
lscpu                   # CPU info
lsmem                   # Memory info
lsblk                   # Block devices
lsusb                   # USB devices
lspci                   # PCI devices

# Disk usage
df -h                   # Filesystem usage
du -sh directory/       # Directory size
iostat                  # I/O statistics

# Memory usage
free -h
vmstat 1                # Virtual memory stats
```

---

## 🔐 Security Essentials

### Firewall Management
```bash
# UFW (Uncomplicated Firewall)
sudo ufw status
sudo ufw enable
sudo ufw allow 22/tcp
sudo ufw deny 80/tcp
sudo ufw delete allow 80/tcp

# iptables
sudo iptables -L                    # List rules
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables-save > /etc/iptables/rules.v4
```

### Service Management
```bash
# systemd
sudo systemctl start service-name
sudo systemctl stop service-name
sudo systemctl restart service-name
sudo systemctl status service-name
sudo systemctl enable service-name      # Start on boot
sudo systemctl disable service-name
```

### File Integrity
```bash
# MD5/SHA checksums
md5sum file.txt
sha256sum file.txt
sha512sum file.txt

# Verify checksum
echo "hash_value  filename" | sha256sum -c
```

---

## 📖 Learning Resources

- **TryHackMe**: Linux Fundamentals rooms (Part 1, 2, 3)
- **OverTheWire**: Bandit wargame
- **Linux Journey**: [https://linuxjourney.com/](https://linuxjourney.com/)
- **The Linux Command Line**: Book by William Shotts
- **Explainshell**: [https://explainshell.com/](https://explainshell.com/)

---

## 🎯 Practice Labs

1. **TryHackMe Rooms**
   - Linux Fundamentals 1, 2, 3
   - Linux Privilege Escalation
   - Linux Strength Training

2. **OverTheWire**
   - Bandit (Basic commands)
   - Leviathan (Permission exploitation)

3. **HackTheBox**
   - Linux machines
   - Linux challenges

---

## 🔗 Next Steps

After mastering Linux fundamentals:
- [Bash Scripting for Security](./Bash-Scripting.md)
- [Linux Privilege Escalation](./Linux-Privilege-Escalation.md)
- [System Hardening](./Linux-Hardening.md)

---

*Master Linux, master cybersecurity. The command line is your most powerful tool.*
