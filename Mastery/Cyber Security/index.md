If DevOps answers:

> **"How do we deploy software safely?"**

Cybersecurity answers:

> **"How can attackers break it, and how do defenders stop them?"**

To become **interview-ready + practically skilled**, don't study tools randomly. Learn them in the same order an attacker or defender would encounter them.

---

# Complete Cybersecurity & Ethical Hacking Roadmap

```text
Computer Fundamentals
        │
        ▼
Networking (Deep)
        │
        ▼
Linux & Windows Internals
        │
        ▼
Programming & Scripting
        │
        ▼
Web Technologies
        │
        ▼
Cryptography
        │
        ▼
Reconnaissance
        │
        ▼
Enumeration
        │
        ▼
Vulnerability Assessment
        │
        ▼
Web Application Security
        │
        ▼
System Exploitation
        │
        ▼
Active Directory
        │
        ▼
Wireless Security
        │
        ▼
Cloud Security
        │
        ▼
Container Security
        │
        ▼
Mobile Security
        │
        ▼
Reverse Engineering
        │
        ▼
Malware Analysis
        │
        ▼
Digital Forensics
        │
        ▼
Incident Response
        │
        ▼
Threat Hunting
        │
        ▼
SOC Operations
        │
        ▼
Red Team
        │
        ▼
Blue Team
```

---

# PHASE 0 — Computer Science Foundation

## Operating Systems

- Process
    
- Thread
    
- Memory
    
- Virtual Memory
    
- Scheduling
    
- File Systems
    
- IPC
    
- Kernel
    
- User Mode
    
- System Calls
    
- Boot Process
    
- Windows Internals
    
- Linux Internals
    

---

## Networking (Master This)

Layer 1–7

TCP

UDP

ARP

ICMP

DNS

DHCP

HTTP

HTTPS

FTP

SSH

SMTP

POP3

IMAP

SNMP

LDAP

SMB

NFS

VPN

TLS

SSL

BGP

OSPF

MPLS

NAT

Firewalls

Load Balancers

Proxies

Reverse Proxies

IPv6

---

# PHASE 1 — Linux

Master

```text
bash

grep

awk

sed

find

curl

wget

netcat

tcpdump

iptables

nmap

journalctl

systemctl

cron

ssh
```

Advanced

Namespaces

Capabilities

SELinux

AppArmor

Kernel Modules

Syscalls

procfs

sysfs

---

# PHASE 2 — Windows

Registry

PowerShell

Active Directory

NTLM

Kerberos

LSASS

Credential Manager

SMB

Windows Services

Group Policy

Event Viewer

Sysinternals

Windows Internals

---

# PHASE 3 — Programming

Python

- sockets
    
- requests
    
- threading
    
- asyncio
    
- regex
    
- cryptography
    
- pwntools
    

C

Memory

Pointers

Stack

Heap

Buffer Overflow basics

Assembly

x86

x64

Registers

Calling Convention

Go (optional)

JavaScript

PHP

SQL

Bash

PowerShell

---

# PHASE 4 — Web Technologies

HTTP

REST

Cookies

JWT

Sessions

CORS

Same Origin

WebSockets

GraphQL

OAuth

OpenID Connect

HTML

CSS

JavaScript

Node

PHP

ASP.NET basics

---

# PHASE 5 — Cryptography (Master Level)

This deserves its own roadmap.

## Foundations

- Number Theory
    
- Modular Arithmetic
    
- Prime Numbers
    
- Euler's Totient
    
- Chinese Remainder Theorem
    
- Finite Fields
    

---

## Classical

Caesar

Vigenere

Hill

Playfair

Rail Fence

Transposition

Frequency Analysis

---

## Hashing

MD5

SHA1

SHA2

SHA3

BLAKE2

BLAKE3

Password Hashing

bcrypt

scrypt

Argon2

Salting

Peppering

Rainbow Tables

---

## Symmetric Encryption

DES

3DES

AES

ChaCha20

Blowfish

Twofish

Serpent

Modes

ECB

CBC

CTR

GCM

XTS

---

## Asymmetric

RSA

ECC

Diffie-Hellman

ECDH

ElGamal

Ed25519

ECDSA

Digital Signatures

Key Exchange

---

## Public Key Infrastructure

Certificates

X509

CSR

CA

Chain of Trust

OCSP

CRL

Certificate Pinning

---

## TLS

TLS Handshake

Cipher Suites

Perfect Forward Secrecy

ALPN

HSTS

---

## Modern Topics

Post-Quantum Cryptography

Hybrid Encryption

Secret Sharing

Zero Knowledge Proofs

Homomorphic Encryption

Secure Multi-Party Computation

---

## Practical Crypto Labs

- Implement AES in Python
    
- RSA encryption/decryption
    
- Build a password manager
    
- Write a TLS packet parser
    
- Analyze TLS with Wireshark
    
- JWT signing/verification
    
- Hash cracking experiments on lab data
    
- Create and validate certificates
    
- Break weak RSA keys in toy examples
    
- Perform padding oracle exercises in safe labs
    

---

# PHASE 6 — Reconnaissance

OSINT

Google Dorks

WHOIS

DNS Enumeration

Subdomain Enumeration

GitHub Enumeration

Email Enumeration

Metadata

Wayback Machine

Shodan

Censys

Amass

Subfinder

Assetfinder

theHarvester

SpiderFoot

---

# PHASE 7 — Scanning & Enumeration

Nmap

Masscan

RustScan

Nikto

Gobuster

Dirsearch

ffuf

Feroxbuster

SMB Enumeration

SNMP Enumeration

LDAP Enumeration

RPC Enumeration

FTP Enumeration

SMTP Enumeration

DNS Zone Transfer

---

# PHASE 8 — Web Security

Master the entire **OWASP Top 10** plus common real-world classes.

- SQL Injection
    
- Cross-Site Scripting (Stored, Reflected, DOM)
    
- CSRF
    
- SSRF
    
- XXE
    
- IDOR
    
- Authentication flaws
    
- Authorization flaws
    
- File Upload
    
- Command Injection
    
- Path Traversal
    
- SSTI
    
- Insecure Deserialization
    
- Business Logic flaws
    
- JWT attacks
    
- OAuth mistakes
    
- CORS
    
- Clickjacking
    
- HTTP Request Smuggling
    
- Cache Poisoning
    
- Race Conditions
    
- Prototype Pollution
    

### Labs

- **PortSwigger Web Security Academy** (complete every topic)
    
- OWASP Juice Shop
    
- DVWA
    
- bWAPP
    
- WebGoat
    
- XVWA
    
- Mutillidae II
    

---

# PHASE 9 — Binary Exploitation

Memory Layout

Stack

Heap

ASLR

DEP/NX

ROP

Stack Overflow

Heap Exploitation

Format String

Use-After-Free

Double Free

Integer Overflow

Labs

- picoCTF
    
- pwn.college
    
- OverTheWire (Behemoth, Utumno, Maze)
    
- ROP Emporium
    

---

# PHASE 10 — Active Directory

Kerberos

NTLM

BloodHound

LDAP

ACL Abuse

Delegation

Golden Ticket

Silver Ticket

Kerberoasting

AS-REP Roasting

Pass-the-Hash

Pass-the-Ticket

Labs

- Hack The Box Active Directory machines
    
- GOAD (Game of Active Directory)
    
- AttackIQ Academy (where available)
    

---

# PHASE 11 — Cloud Security

AWS

IAM

S3

Lambda

EC2

VPC

EKS

CloudTrail

GuardDuty

Azure

Entra ID (Azure AD)

Microsoft Defender

GCP IAM

Labs

- CloudGoat
    
- flaws.cloud
    
- IAM challenges
    
- LocalStack experiments
    

---

# PHASE 12 — Container & Kubernetes Security

Docker

Namespaces

Capabilities

Seccomp

AppArmor

SELinux

Image Scanning

Trivy

Falco

RBAC

Admission Controllers

Network Policies

Kubernetes Secrets

Labs

- Kubernetes Goat
    
- KubeGoat
    
- kube-hunter
    
- kube-bench
    

---

# PHASE 13 — Wireless

802.11

WPA2

WPA3

Handshake

PMKID

Evil Twin

Captive Portal

Bluetooth basics

Labs

- aircrack-ng suite
    
- WiFi Pineapple (if available)
    
- ESP32-based lab projects
    

---

# PHASE 14 — Reverse Engineering

PE

ELF

Assembly

Ghidra

IDA Free

Binary Ninja (optional)

Radare2

x64dbg

WinDbg

Labs

- crackmes.one
    
- Malware Unicorn exercises
    
- FLARE-ON (when available)
    

---

# PHASE 15 — Malware Analysis

Static

Dynamic

Behavior

Persistence

Packing

Obfuscation

YARA

CAPA

Labs

- REMnux
    
- FLARE-VM
    
- Any.Run (public samples)
    
- Malware-Traffic-Analysis.net PCAPs
    

---

# PHASE 16 — Digital Forensics

Disk

Memory

Network

Timeline

Registry

Browser

Email

Tools

- Autopsy
    
- Volatility 3
    
- Plaso
    
- Wireshark
    
- FTK Imager (community use)
    

---

# PHASE 17 — SOC & Detection Engineering

SIEM

Splunk

Elastic

Microsoft Sentinel

QRadar (concepts)

Detection Rules

Sigma

YARA

MITRE ATT&CK

Threat Intelligence

Incident Response

Threat Hunting

Labs

- DetectionLab
    
- Security Onion
    
- Splunk Boss of the SOC datasets
    

---

# PHASE 18 — Red Team

Initial Access

Privilege Escalation

Lateral Movement

Persistence

Defense Evasion

Credential Access

Command & Control

Exfiltration

Report Writing

Purple Team exercises

---

# PHASE 19 — Certifications (Optional)

Foundational:

- Google Cybersecurity Certificate
    
- CompTIA Security+
    
- CompTIA Network+
    

Intermediate:

- PNPT
    
- eJPT
    
- Security Blue Team Level 1 (BTL1)
    

Advanced:

- CRTO
    
- OSCP
    
- OSEP
    
- CRTP
    
- CARTP
    
- CCD
    
- CISSP (later in your career)
    

---

# Best Hands-On Platforms

## Beginner → Intermediate

- PortSwigger Web Security Academy (complete all labs)
    
- OverTheWire
    
- picoCTF
    
- TryHackMe
    
- OWASP Juice Shop
    
- DVWA
    
- WebGoat
    
- Security Blue Team Labs
    

## Intermediate → Advanced

- Hack The Box
    
- Hack The Box Academy
    
- VulnHub
    
- Proving Grounds
    
- GOAD
    
- pwn.college
    
- ROP Emporium
    
- CyberDefenders
    
- Blue Team Labs Online
    

## Advanced

- CRTO labs
    
- OSCP labs
    
- DetectionLab
    
- CloudGoat
    
- Kubernetes Goat
    
- FLARE-ON
    
- Malware-Traffic-Analysis.net
    

---

# Recommended Home Lab

```text
Windows 11 Host
│
├── VMware / VirtualBox
│
├── Kali Linux
├── Ubuntu Server
├── Windows Server
├── Windows 11 Client
├── Active Directory Domain
├── Metasploitable 2
├── OWASP Juice Shop
├── DVWA
├── Security Onion
├── Docker Lab
├── Kubernetes (kind or k3d)
├── Splunk Free / Elastic
└── REMnux
```

---

# Suggested Learning Order

|Phase|Focus|
|---|---|
|1|Linux + Windows Internals|
|2|Networking|
|3|Python, Bash, PowerShell, C basics|
|4|Web Technologies|
|5|**Cryptography (deep)**|
|6|Recon & Enumeration|
|7|Web Application Security (PortSwigger first)|
|8|Binary Exploitation|
|9|Active Directory|
|10|Cloud Security|
|11|Containers & Kubernetes Security|
|12|Reverse Engineering & Malware Analysis|
|13|Digital Forensics & Incident Response|
|14|SOC, Detection Engineering & Threat Hunting|
|15|Advanced Red Team Operations|

## Since you've already studied Linux, networking, DevOps basics, Docker, Kubernetes, and have prior web application assessment experience, you can move through the fundamentals faster and spend more time on the hands-on labs. A strong progression for you would be: **PortSwigger Web Security Academy → Hack The Box Academy → Active Directory labs (GOAD) → CloudGoat → DetectionLab**, while studying cryptography in parallel rather than leaving it until the end. This sequence builds practical offensive and defensive skills on top of the foundation you've already established.