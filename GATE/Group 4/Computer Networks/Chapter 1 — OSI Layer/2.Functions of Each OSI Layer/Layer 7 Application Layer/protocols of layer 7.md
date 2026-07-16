# Layer 7 — Application Layer Protocols

|Protocol|Full Form|Purpose|Default Port|
|---|---|---|---|
|**HTTP**|HyperText Transfer Protocol|Transfers web pages|80|
|**HTTPS**|HyperText Transfer Protocol Secure|Secure web communication (HTTP + TLS)|443|
|**FTP**|File Transfer Protocol|Transfers files|20 (Data), 21 (Control)|
|**TFTP**|Trivial File Transfer Protocol|Simple file transfer|69 (UDP)|
|**SMTP**|Simple Mail Transfer Protocol|Sends emails|25, 587, 465|
|**POP3**|Post Office Protocol v3|Downloads emails|110, 995|
|**IMAP**|Internet Message Access Protocol|Synchronizes emails|143, 993|
|**DNS**|Domain Name System|Converts domain names to IP addresses|53|
|**DHCP**|Dynamic Host Configuration Protocol|Automatically assigns IP addresses|67/68 (UDP)|
|**Telnet**|Telecommunication Network|Remote login (insecure)|23|
|**SSH**|Secure Shell|Secure remote login|22|
|**SNMP**|Simple Network Management Protocol|Network monitoring and management|161/162|
|**NTP**|Network Time Protocol|Time synchronization|123 (UDP)|
|**LDAP**|Lightweight Directory Access Protocol|Directory services (e.g., Active Directory)|389, 636|
|**SIP**|Session Initiation Protocol|Voice/Video call setup (VoIP)|5060, 5061|
|**RTP**|Real-time Transport Protocol|Carries audio/video streams|Dynamic UDP|
|**RTSP**|Real Time Streaming Protocol|Controls media streaming|554|

---

# ⭐ High-Priority GATE Protocols

These are the ones GATE asks about most often:

- HTTP
- HTTPS
- FTP
- DNS
- DHCP
- SMTP
- POP3
- IMAP
- SSH
- Telnet
- SNMP

---

# Not Part of Layer 7 (Common Confusion)

|Protocol|Actual Layer|
|---|---|
|TCP|Transport Layer|
|UDP|Transport Layer|
|IP|Network Layer|
|ICMP|Network Layer|
|ARP|Data Link Layer|
|Ethernet|Data Link Layer|

> **Exam Tip:** Many students mistakenly classify **TCP/IP** as Application Layer protocols. Remember: **HTTP uses TCP**, but **HTTP is not TCP**.