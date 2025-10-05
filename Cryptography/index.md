---
title: "Cryptography Basics for Security"
permalink: /Cryptography/
layout: single
author_profile: true
toc: true
toc_sticky: true
---

> Essential cryptography concepts for cybersecurity professionals

---

## 📋 Overview

Cryptography is the practice of securing communication and data through the use of codes and ciphers. Understanding cryptography is essential for cybersecurity professionals, whether you're testing systems or building secure applications.

**Why Learn Cryptography?**
- 🔒 Understand how data protection works
- 🔑 Learn to identify weak implementations
- 🛡️ Build secure systems
- 🔍 Break weak crypto in CTFs and pentests
- 📊 Understand encryption standards

---

## 🎯 Core Concepts

### The CIA Triad

**Confidentiality**: Only authorized parties can access data
- Achieved through encryption
- Access controls

**Integrity**: Data hasn't been tampered with
- Achieved through hashing
- Digital signatures

**Availability**: Data is accessible when needed
- Redundancy
- DDoS protection

### Encryption vs. Encoding vs. Hashing

**Encryption**
- Two-way function (reversible)
- Requires a key
- Purpose: Confidentiality
- Example: AES, RSA

**Encoding**
- Two-way function (reversible)
- No key required
- Purpose: Data representation
- Example: Base64, URL encoding
- **NOT for security!**

**Hashing**
- One-way function (irreversible)
- No key required
- Purpose: Integrity, password storage
- Example: SHA-256, bcrypt

---

## 🔐 Types of Cryptography

### 1. Symmetric Encryption
Same key for encryption and decryption.

**Common Algorithms:**
- **AES** (Advanced Encryption Standard) - Industry standard
- **DES** (Data Encryption Standard) - Obsolete, use AES
- **3DES** (Triple DES) - More secure than DES, but slow
- **ChaCha20** - Modern alternative to AES

**Example: AES with Python**
```python
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

# Encryption
key = get_random_bytes(32)  # 256-bit key
cipher = AES.new(key, AES.MODE_CBC)
plaintext = b"Secret message"
ciphertext = cipher.encrypt(pad(plaintext, AES.block_size))

# Decryption
cipher = AES.new(key, AES.MODE_CBC, cipher.iv)
decrypted = unpad(cipher.decrypt(ciphertext), AES.block_size)
```

**Pros:**
✅ Fast
✅ Efficient for large data

**Cons:**
❌ Key distribution problem
❌ Same key for all operations

---

### 2. Asymmetric Encryption
Different keys for encryption (public) and decryption (private).

**Common Algorithms:**
- **RSA** - Most widely used
- **ECC** (Elliptic Curve Cryptography) - More efficient than RSA
- **DSA** (Digital Signature Algorithm)
- **Diffie-Hellman** - Key exchange

**Example: RSA with Python**
```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP

# Generate key pair
key = RSA.generate(2048)
public_key = key.publickey()

# Encryption (with public key)
cipher = PKCS1_OAEP.new(public_key)
ciphertext = cipher.encrypt(b"Secret message")

# Decryption (with private key)
cipher = PKCS1_OAEP.new(key)
plaintext = cipher.decrypt(ciphertext)
```

**Pros:**
✅ Solves key distribution
✅ Digital signatures
✅ Key exchange

**Cons:**
❌ Slower than symmetric
❌ Limited data size

---

### 3. Hash Functions
One-way functions that produce fixed-size outputs.

**Common Algorithms:**
- **SHA-256** - Secure, widely used
- **SHA-1** - Deprecated (collisions found)
- **MD5** - Broken, don't use for security
- **bcrypt** - Password hashing
- **Argon2** - Modern password hashing

**Example: Hashing**
```python
import hashlib

# Simple hashing
text = b"password123"
hash_object = hashlib.sha256(text)
hex_dig = hash_object.hexdigest()
print(f"SHA-256: {hex_dig}")

# Password hashing with bcrypt
import bcrypt
password = b"super_secret"
hashed = bcrypt.hashpw(password, bcrypt.gensalt())

# Verify password
if bcrypt.checkpw(password, hashed):
    print("Password matches!")
```

**Use Cases:**
- Password storage
- Data integrity verification
- Digital signatures
- Proof of work (blockchain)

---

## 🔑 Classical Ciphers

### Caesar Cipher
Shift each letter by a fixed number.

```python
def caesar_encrypt(text, shift):
    result = ""
    for char in text:
        if char.isalpha():
            ascii_offset = ord('A') if char.isupper() else ord('a')
            result += chr((ord(char) - ascii_offset + shift) % 26 + ascii_offset)
        else:
            result += char
    return result

# Encrypt
plaintext = "HELLO"
encrypted = caesar_encrypt(plaintext, 3)  # "KHOOR"

# Decrypt
decrypted = caesar_encrypt(encrypted, -3)  # "HELLO"
```

### Vigenère Cipher
Uses a keyword for multiple Caesar shifts.

```python
def vigenere_encrypt(text, key):
    result = ""
    key = key.upper()
    key_index = 0
    
    for char in text:
        if char.isalpha():
            shift = ord(key[key_index % len(key)]) - ord('A')
            ascii_offset = ord('A') if char.isupper() else ord('a')
            result += chr((ord(char) - ascii_offset + shift) % 26 + ascii_offset)
            key_index += 1
        else:
            result += char
    
    return result
```

### XOR Cipher
Simple but powerful when used correctly.

```python
def xor_encrypt(plaintext, key):
    return bytes([p ^ k for p, k in zip(plaintext, key * (len(plaintext) // len(key) + 1))])

plaintext = b"SECRET"
key = b"KEY"
encrypted = xor_encrypt(plaintext, key)
decrypted = xor_encrypt(encrypted, key)  # Back to plaintext
```

---

## 🛠️ Cryptographic Tools

### Command Line Tools

**OpenSSL**
```bash
# Generate RSA key pair
openssl genrsa -out private.pem 2048
openssl rsa -in private.pem -pubout -out public.pem

# Encrypt file
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc

# Decrypt file
openssl enc -d -aes-256-cbc -in file.enc -out file.txt

# Generate hash
echo -n "password" | openssl dgst -sha256

# Generate certificate
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365
```

**GPG (GNU Privacy Guard)**
```bash
# Generate key pair
gpg --gen-key

# Encrypt file
gpg -e -r recipient@email.com file.txt

# Decrypt file
gpg -d file.txt.gpg

# Sign file
gpg --sign file.txt

# Verify signature
gpg --verify file.txt.gpg
```

**Hash Commands**
```bash
# MD5 (don't use for security)
md5sum file.txt

# SHA-256
sha256sum file.txt

# Compare hashes
echo "hash_value  file.txt" | sha256sum -c
```

---

## 🔍 Breaking Weak Crypto

### Frequency Analysis
For simple substitution ciphers.

```python
def frequency_analysis(text):
    freq = {}
    text = text.upper()
    
    for char in text:
        if char.isalpha():
            freq[char] = freq.get(char, 0) + 1
    
    # Sort by frequency
    sorted_freq = sorted(freq.items(), key=lambda x: x[1], reverse=True)
    return sorted_freq

# In English, most common letters: E, T, A, O, I, N
```

### Brute Force
For small keyspaces like Caesar cipher.

```python
def brute_force_caesar(ciphertext):
    for shift in range(26):
        decrypted = caesar_encrypt(ciphertext, -shift)
        print(f"Shift {shift}: {decrypted}")
```

### Dictionary Attacks
For weak passwords or keys.

```python
def dictionary_attack(hash_to_crack, wordlist_file):
    with open(wordlist_file, 'r') as f:
        for word in f:
            word = word.strip()
            if hashlib.sha256(word.encode()).hexdigest() == hash_to_crack:
                return word
    return None
```

---

## 🎓 Learning Path

### Week 1-2: Fundamentals
- [ ] Understand encryption vs encoding vs hashing
- [ ] Learn classical ciphers (Caesar, Vigenère)
- [ ] Practice with CyberChef and online tools
- [ ] Complete [CryptoHack](https://cryptohack.org/) beginner challenges

### Week 3-4: Modern Cryptography
- [ ] Study symmetric encryption (AES)
- [ ] Learn asymmetric encryption (RSA)
- [ ] Understand hash functions
- [ ] Practice with OpenSSL

### Week 5-6: Applied Cryptography
- [ ] SSL/TLS and HTTPS
- [ ] Digital signatures
- [ ] Password storage best practices
- [ ] Certificate management

### Week 7-8: Cryptanalysis
- [ ] Frequency analysis
- [ ] Brute force techniques
- [ ] Known plaintext attacks
- [ ] CTF crypto challenges

---

## 🎯 Common Vulnerabilities

### Weak Implementations
- Using MD5 or SHA-1 for security
- ECB mode for block ciphers
- Weak password hashing
- Small key sizes
- Predictable IVs (Initialization Vectors)

### Improper Usage
```python
# ❌ BAD: ECB mode shows patterns
cipher = AES.new(key, AES.MODE_ECB)

# ✅ GOOD: Use CBC or GCM
cipher = AES.new(key, AES.MODE_CBC)

# ❌ BAD: Weak password hashing
hash = hashlib.md5(password.encode()).hexdigest()

# ✅ GOOD: Use bcrypt or Argon2
hash = bcrypt.hashpw(password, bcrypt.gensalt())
```

---

## 📚 Resources

### Online Platforms
- **[CryptoHack](https://cryptohack.org/)** - Interactive crypto challenges
- **[Cryptopals](https://cryptopals.com/)** - Crypto challenges
- **[CyberChef](https://gchq.github.io/CyberChef/)** - Crypto operations tool

### Books
- "Cryptography Engineering" by Ferguson, Schneier, and Kohno
- "Serious Cryptography" by Jean-Philippe Aumasson
- "Applied Cryptography" by Bruce Schneier

### Tools
- **CyberChef** - Web-based crypto operations
- **OpenSSL** - Command-line crypto toolkit
- **hashcat** - Password cracking
- **John the Ripper** - Password cracking

---

## 🔗 Next Steps

After mastering cryptography basics:
1. **[Web Security](../Web-Security/)** - Apply crypto to web apps
2. **[CTF Guide](../CTF/)** - Practice crypto challenges
3. **[Scripting](../Scripting/)** - Automate crypto tasks

---

## 📖 Detailed Guide

For comprehensive cryptography concepts and examples:

**[View Complete Cryptography Guide](./Cryptography-Basics/)**

---

<div class="notice--warning">
  <h4>⚠️ Important Note</h4>
  <p><strong>Never roll your own crypto!</strong> Use well-established, peer-reviewed cryptographic libraries and implementations. Cryptography is easy to get wrong, and mistakes can be catastrophic. Always use standard libraries like OpenSSL, libsodium, or language-specific crypto libraries.</p>
</div>

---

*"In cryptography, a little knowledge is a dangerous thing."* 🔐
