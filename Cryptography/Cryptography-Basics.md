# 🔐 Cryptography Basics for Security

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

### 1. Confidentiality, Integrity, and Availability (CIA Triad)

**Confidentiality**: Only authorized parties can access data
- Achieved through encryption
- Access controls

**Integrity**: Data hasn't been tampered with
- Achieved through hashing
- Digital signatures

**Availability**: Data is accessible when needed
- Redundancy
- DDoS protection

### 2. Encryption vs. Encoding vs. Hashing

**Encryption**
```
- Two-way function (reversible)
- Requires a key
- Purpose: Confidentiality
- Example: AES, RSA
```

**Encoding**
```
- Two-way function (reversible)
- No key required
- Purpose: Data representation
- Example: Base64, URL encoding
- NOT for security!
```

**Hashing**
```
- One-way function (irreversible)
- No key required
- Purpose: Integrity, password storage
- Example: SHA-256, bcrypt
```

---

## 🔑 Types of Cryptography

### 1. Symmetric Encryption

**Same key for encryption and decryption**

**Common Algorithms:**
```
AES (Advanced Encryption Standard)
- Block size: 128 bits
- Key sizes: 128, 192, 256 bits
- Most widely used

DES (Data Encryption Standard)
- Block size: 64 bits
- Key size: 56 bits
- DEPRECATED - too weak

3DES (Triple DES)
- Applies DES three times
- More secure than DES
- Slower, being phased out

ChaCha20
- Stream cipher
- Fast, secure
- Used in modern applications
```

**Example (Python):**
```python
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

# Generate key
key = get_random_bytes(16)  # 128-bit key

# Create cipher
cipher = AES.new(key, AES.MODE_CBC)

# Encrypt
plaintext = b"Secret message"
ciphertext = cipher.encrypt(pad(plaintext, AES.block_size))
iv = cipher.iv

# Decrypt
decipher = AES.new(key, AES.MODE_CBC, iv)
decrypted = unpad(decipher.decrypt(ciphertext), AES.block_size)
```

**Use Cases:**
- File encryption
- Disk encryption
- VPN tunnels
- Database encryption

### 2. Asymmetric Encryption

**Different keys for encryption (public) and decryption (private)**

**Common Algorithms:**
```
RSA (Rivest-Shamir-Adleman)
- Key sizes: 2048, 3072, 4096 bits
- Most common asymmetric algorithm
- Slower than symmetric

ECC (Elliptic Curve Cryptography)
- Smaller keys, same security
- More efficient
- Used in mobile devices

DSA (Digital Signature Algorithm)
- For digital signatures
- Not for encryption
```

**RSA Example (Python):**
```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP

# Generate key pair
key = RSA.generate(2048)
public_key = key.publickey()

# Encrypt with public key
cipher = PKCS1_OAEP.new(public_key)
ciphertext = cipher.encrypt(b"Secret message")

# Decrypt with private key
decipher = PKCS1_OAEP.new(key)
plaintext = decipher.decrypt(ciphertext)
```

**Use Cases:**
- SSL/TLS certificates
- SSH authentication
- Digital signatures
- Key exchange

### 3. Hash Functions

**One-way functions that produce fixed-size output**

**Common Hash Functions:**
```
SHA-256 (Secure Hash Algorithm)
- Output: 256 bits
- Widely used, secure
- Part of SHA-2 family

SHA-1
- Output: 160 bits
- DEPRECATED - collisions found
- Don't use for security

MD5
- Output: 128 bits
- DEPRECATED - broken
- Only for checksums, not security

bcrypt
- Designed for passwords
- Includes salt
- Adaptive (can increase rounds)

Argon2
- Modern password hashing
- Winner of PHC competition
- Resistant to GPU attacks
```

**Hashing Example:**
```python
import hashlib

# SHA-256
message = b"Hello, World!"
hash_object = hashlib.sha256(message)
hex_dig = hash_object.hexdigest()
print(hex_dig)

# MD5 (for non-security purposes only)
hash_object = hashlib.md5(message)
hex_dig = hash_object.hexdigest()
```

**Password Hashing:**
```python
import bcrypt

# Hash password
password = b"super_secret"
salt = bcrypt.gensalt()
hashed = bcrypt.hashpw(password, salt)

# Verify password
if bcrypt.checkpw(password, hashed):
    print("Password matches!")
```

**Use Cases:**
- Password storage
- File integrity verification
- Digital signatures
- Blockchain
- Certificate fingerprints

---

## 🔐 Block Cipher Modes

### ECB (Electronic Codebook)
```
❌ INSECURE - Don't use!
- Same plaintext → same ciphertext
- Patterns visible
- No IV needed
```

### CBC (Cipher Block Chaining)
```
✅ Good for general use
- Each block depends on previous
- Requires IV (Initialization Vector)
- Padding required
```

### CTR (Counter)
```
✅ Good for parallel processing
- Turns block cipher into stream cipher
- No padding needed
- Requires nonce/counter
```

### GCM (Galois/Counter Mode)
```
✅ Best for authenticated encryption
- Provides confidentiality and integrity
- Used in TLS 1.3
- No padding needed
```

**Example:**
```python
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes

key = get_random_bytes(16)

# CBC Mode
cipher_cbc = AES.new(key, AES.MODE_CBC)
ciphertext = cipher_cbc.encrypt(pad(data, AES.block_size))

# GCM Mode (authenticated encryption)
cipher_gcm = AES.new(key, AES.MODE_GCM)
ciphertext, tag = cipher_gcm.encrypt_and_digest(data)
```

---

## 🛡️ Digital Signatures

**Ensure authenticity and integrity of messages**

**How They Work:**
```
1. Hash the message
2. Encrypt hash with private key (sign)
3. Recipient decrypts with public key (verify)
4. Compare hash with message hash
```

**Example:**
```python
from Crypto.PublicKey import RSA
from Crypto.Signature import pkcs1_15
from Crypto.Hash import SHA256

# Generate keys
key = RSA.generate(2048)

# Sign
message = b"Important message"
h = SHA256.new(message)
signature = pkcs1_15.new(key).sign(h)

# Verify
try:
    pkcs1_15.new(key.publickey()).verify(h, signature)
    print("Signature is valid!")
except:
    print("Signature is invalid!")
```

---

## 🔍 Common Cryptographic Attacks

### 1. Brute Force Attack
```
Try all possible keys
- Effective against weak keys
- Time-consuming for strong crypto
```

**Defense:**
- Use strong keys (AES-256, RSA-2048+)
- Implement rate limiting

### 2. Dictionary Attack
```
Try common passwords/keys
- Fast with good wordlists
- Effective against weak passwords
```

**Tools:**
```bash
# John the Ripper
john --wordlist=rockyou.txt hash.txt

# Hashcat
hashcat -m 0 -a 0 hash.txt wordlist.txt
```

**Defense:**
- Strong, unique passwords
- Password policies
- Account lockout

### 3. Rainbow Table Attack
```
Precomputed hash tables
- Fast lookup
- Effective against unsalted hashes
```

**Defense:**
- Use salted hashes
- Use bcrypt, Argon2

### 4. Man-in-the-Middle (MITM)
```
Intercept communication
- Read/modify messages
- Impersonate parties
```

**Defense:**
- Use TLS/SSL
- Certificate pinning
- End-to-end encryption

### 5. Padding Oracle Attack
```
Exploit padding validation
- Decrypt CBC mode ciphertext
- Without knowing the key
```

**Defense:**
- Use authenticated encryption (GCM)
- Don't reveal padding errors

### 6. Timing Attack
```
Measure execution time
- Determine secrets from time differences
- Side-channel attack
```

**Defense:**
- Constant-time operations
- Use secure comparison functions

---

## 🧪 Practical Cryptography Tools

### OpenSSL
```bash
# Generate RSA key
openssl genrsa -out private.key 2048
openssl rsa -in private.key -pubout -out public.key

# Encrypt/decrypt with RSA
openssl rsautl -encrypt -inkey public.key -pubin -in plaintext.txt -out encrypted.txt
openssl rsautl -decrypt -inkey private.key -in encrypted.txt -out decrypted.txt

# AES encryption
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc
openssl enc -d -aes-256-cbc -in file.enc -out file.txt

# Generate hash
echo -n "text" | openssl dgst -sha256

# Generate self-signed certificate
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365
```

### Python Cryptography Libraries
```python
# PyCryptodome
from Crypto.Cipher import AES
from Crypto.PublicKey import RSA
from Crypto.Hash import SHA256

# cryptography library
from cryptography.fernet import Fernet
from cryptography.hazmat.primitives.asymmetric import rsa

# Example with Fernet (symmetric encryption)
from cryptography.fernet import Fernet

# Generate key
key = Fernet.generate_key()
cipher = Fernet(key)

# Encrypt
ciphertext = cipher.encrypt(b"Secret message")

# Decrypt
plaintext = cipher.decrypt(ciphertext)
```

### John the Ripper
```bash
# Crack password hashes
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

# Show cracked passwords
john --show hash.txt

# Crack with rules
john --wordlist=wordlist.txt --rules hash.txt
```

### Hashcat
```bash
# MD5
hashcat -m 0 -a 0 hash.txt wordlist.txt

# SHA-256
hashcat -m 1400 -a 0 hash.txt wordlist.txt

# bcrypt
hashcat -m 3200 -a 0 hash.txt wordlist.txt

# With rules
hashcat -m 0 -a 0 hash.txt wordlist.txt -r rules/best64.rule
```

---

## 🎓 CTF Crypto Challenges

### Classical Ciphers

**Caesar Cipher**
```python
def caesar_decrypt(ciphertext, shift):
    result = ""
    for char in ciphertext:
        if char.isalpha():
            ascii_offset = 65 if char.isupper() else 97
            result += chr((ord(char) - ascii_offset - shift) % 26 + ascii_offset)
        else:
            result += char
    return result

# Try all shifts
ciphertext = "KHOOR"
for shift in range(26):
    print(f"Shift {shift}: {caesar_decrypt(ciphertext, shift)}")
```

**Base64**
```python
import base64

# Decode
encoded = "SGVsbG8gV29ybGQh"
decoded = base64.b64decode(encoded)
print(decoded)  # b'Hello World!'

# Encode
plaintext = b"Secret"
encoded = base64.b64encode(plaintext)
```

**XOR**
```python
def xor_decrypt(ciphertext, key):
    return bytes([c ^ key for c in ciphertext])

# Single byte XOR
ciphertext = b'\x1f\x0e\x0c\x0c\x00'
for key in range(256):
    plaintext = xor_decrypt(ciphertext, key)
    if plaintext.isascii():
        print(f"Key {key}: {plaintext}")
```

### RSA Attacks

**Small e Attack**
```python
# If e is small (e.g., e=3) and message^e < n
# Can simply take eth root

import gmpy2

e = 3
c = 123456789  # ciphertext

# Calculate eth root
m = gmpy2.iroot(c, e)[0]
print(f"Plaintext: {m}")
```

**Common Modulus Attack**
```python
# When same message sent with same n but different e
# Can recover message without private key
```

---

## 📚 Best Practices

### For Developers

**DO:**
```
✅ Use established libraries (don't roll your own crypto)
✅ Use AES-256 for symmetric encryption
✅ Use RSA-2048 or higher for asymmetric
✅ Use bcrypt or Argon2 for passwords
✅ Use TLS 1.2 or higher
✅ Use authenticated encryption (GCM)
✅ Keep keys secure (HSM, key management)
✅ Rotate keys regularly
```

**DON'T:**
```
❌ Use MD5 or SHA-1 for security
❌ Use ECB mode
❌ Store passwords in plain text
❌ Use custom/homebrew crypto
❌ Reuse IVs/nonces
❌ Hard-code keys in source
❌ Use weak key sizes
```

### For Pentesters

**Check For:**
```
- Weak encryption algorithms (DES, RC4)
- Weak hashing (MD5, SHA-1)
- Hard-coded keys/secrets
- Insecure random number generation
- Lack of salt in password hashing
- Improper certificate validation
- Weak key sizes
- Insecure cipher modes (ECB)
```

---

## 🔗 Learning Resources

### Interactive Learning
- **CryptoHack**: [https://cryptohack.org/](https://cryptohack.org/)
- **CryptoPals**: [https://cryptopals.com/](https://cryptopals.com/)
- **TryHackMe**: Crypto rooms

### Books
- **Serious Cryptography** by Jean-Philippe Aumasson
- **Applied Cryptography** by Bruce Schneier
- **Cryptography Engineering** by Ferguson, Schneier, Kohno

### Online Courses
- Coursera: Cryptography I (Stanford)
- Khan Academy: Cryptography
- Cybrary: Cryptography courses

### Tools
- **CyberChef**: [https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/)
- **dCode**: [https://www.dcode.fr/](https://www.dcode.fr/)
- **Hash Analyzer**: Online hash identification tools

---

*Encryption protects data, but implementation matters. Use proven libraries, follow best practices.*
