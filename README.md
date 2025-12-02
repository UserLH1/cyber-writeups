# CTF Write-ups Collection

A comprehensive collection of Capture The Flag (CTF) challenge solutions documenting various cybersecurity techniques and methodologies.

## 📚 Overview

This repository contains detailed write-ups for CTF challenges across multiple security domains, showcasing practical skills in exploitation, analysis, and problem-solving.

## 🗂️ Categories

### [Forensics](docs/Forensics.md)
Digital forensics challenges involving:
- **Alternate Data Streams (ADS)** - NTFS hidden streams analysis
- **File Carving** - Recovering deleted files from disk images
- **Memory Analysis** - Volatility framework for RAM dumps
- **Network Forensics** - PCAP analysis and traffic inspection
- **Steganography** - LSB analysis with tools like `zsteg`

### [Web Security](docs/Web_Security.md)
Web application security challenges including:
- **Command Injection** - Exploiting system command vulnerabilities
- **SQL Injection** - Database exploitation and data exfiltration
- **Cross-Site Scripting (XSS)** - Stored XSS and admin impersonation
- **File Upload Vulnerabilities** - Bypassing filters and restrictions
- **JWT Attacks** - Token forging and secret cracking
- **Directory Traversal** - Path manipulation attacks
- **WAF Bypass** - Evading Web Application Firewalls

### [Reverse Engineering](docs/Reverse_Engineering.md)
Binary analysis and game hacking:
- **Memory Corruption** - Game memory manipulation with Cheat Engine
- **.NET Decompilation** - dnSpy for Unity game analysis
- **Static Analysis** - objdump and disassembly techniques
- **Library Call Tracing** - ltrace for dynamic analysis
- **PRNG Exploitation** - Predictable random number generation

### [Blockchain & Cryptography](docs/Blockchain_Crypto.md)
Smart contract and cryptographic challenges:
- **EVM Bytecode Analysis** - Ethereum smart contract decompilation
- **Mathematical Cryptanalysis** - Solving equations for contract exploitation

### [Miscellaneous & Network](docs/Misc_Network.md)
Network protocols and general challenges:
- **Network Connectivity** - Netcat and firewall bypass techniques
- **Protocol Analysis** - Traffic inspection and manipulation

## 🛠️ Tools & Technologies

### Network Analysis
- **Wireshark** - Packet capture and protocol analysis
- **Nmap** - Network scanning and reconnaissance
- **Burp Suite** - Web application security testing
- **Netcat** - Network connectivity and data transfer
- **OWASP ZAP** - Automated web security scanning

### Forensics
- **Volatility 3** - Memory forensics framework
- **Foremost** - File carving tool
- **ExifTool** - Metadata extraction
- **zsteg** - Steganography detection
- **7-Zip** - Archive analysis

### Reverse Engineering
- **Ghidra** - Software reverse engineering suite
- **dnSpy** - .NET assembly editor and debugger
- **Cheat Engine** - Memory scanning and debugging
- **ltrace** - Library call tracer
- **objdump** - Object file disassembler

### Web Exploitation
- **SQLMap** - Automated SQL injection tool
- **Python (Requests)** - Custom exploit scripting
- **Browser DevTools** - Client-side analysis
- **John the Ripper** - Password and hash cracking

### Blockchain
- **Dedaub** - EVM bytecode decompiler
- **Web3 Tools** - Smart contract interaction

## 🚀 Getting Started

### Prerequisites
```bash
# Kali Linux or similar security-focused distribution recommended
# Install common tools:
sudo apt update
sudo apt install volatility3 foremost wireshark sqlmap netcat john
```

### Viewing Write-ups



This documentation is built with **MkDocs**. To view locally:

```bash
# Install MkDocs
pip install mkdocs mkdocs-material

# Serve documentation locally
mkdocs serve

# Build static site
mkdocs build
```

Visit `http://localhost:8000` to browse the write-ups.

## 📖 Documentation Structure

```
.
├── docs/
│   ├── index.md                    # Homepage
│   ├── Forensics.md               # Forensics challenges
│   ├── Web_Security.md            # Web exploitation
│   ├── Reverse_Engineering.md     # Binary analysis
│   ├── Blockchain_Crypto.md       # Smart contracts & crypto
│   └── Misc_Network.md            # Network & misc challenges
├── site/                          # Generated static site
├── mkdocs.yml                     # MkDocs configuration
└── README.md                      # This file
```

## 🎯 Key Techniques Demonstrated

- **NTFS Alternate Data Streams** enumeration
- **File carving** from disk images
- **Memory dump analysis** with Volatility
- **Command injection** exploitation
- **SQL injection** (UNION-based, time-based)
- **XSS attacks** (stored, DOM-based)
- **File upload filter bypass** (magic bytes, extension manipulation)
- **JWT token** forgery and secret cracking
- **TAR command injection** via wildcards
- **Directory traversal** attacks
- **Steganography** techniques (LSB analysis)
- **EVM smart contract** reverse engineering
- **Game memory** manipulation
- **.NET decompilation** and static analysis

## 🏆 Achievements

- **12+ Web Security** challenges solved
- **4 Forensics** investigations completed
- **3 Reverse Engineering** challenges cracked
- **1 Blockchain** smart contract exploited
- Multiple **network analysis** scenarios

## ⚠️ Disclaimer

**These write-ups are for educational purposes only.** 

All techniques documented here should only be used in:
- Authorized CTF competitions
- Personal learning environments
- Legal penetration testing with proper authorization

Unauthorized access to computer systems is illegal and unethical.

## 📧 Contact

For questions, collaboration, or discussions about these write-ups, feel free to reach out.

## 📄 License

This documentation is shared for educational purposes. Please respect intellectual property and competition rules when referencing these solutions.

---

*"Learning security by breaking things (legally)"*