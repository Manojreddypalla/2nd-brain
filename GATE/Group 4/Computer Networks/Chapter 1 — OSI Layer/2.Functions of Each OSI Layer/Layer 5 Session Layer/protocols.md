## Layer 5 (Session Layer) Protocols

|Protocol|Full Form|Purpose|Status|
|---|---|---|---|
|**NetBIOS**|Network Basic Input/Output System|Establishes and manages sessions in LANs|Legacy|
|**RPC**|Remote Procedure Call|Allows execution of functions on remote computers|Still widely used|
|**PPTP**|Point-to-Point Tunneling Protocol|VPN session establishment|Legacy/Deprecated|
|**SMB Session Service**|Server Message Block Session Service|Manages Windows file-sharing sessions|Used (SMB)|
|**SAP**|Session Announcement Protocol|Announces multimedia sessions|Rare|
|**RTCP**|Real-time Transport Control Protocol|Session control and synchronization for RTP streams|Used in multimedia|
|**H.245**|H.245 Control Protocol|Session control for H.323 multimedia communication|Legacy|
|**SCP**|Session Control Protocol|Multimedia session management (limited use)|Rare|

---

# Most Important Ones (GATE + Interviews)

These are the ones worth remembering:

- ✅ NetBIOS
- ✅ RPC
- ✅ SMB Session Service
- ✅ PPTP

---

# Protocols Often Confused with Layer 5

These are **NOT** Session Layer protocols.

|Protocol|Actual Layer|
|---|---|
|HTTP|Application|
|HTTPS|Application|
|DNS|Application|
|FTP|Application|
|SMTP|Application|
|TCP|Transport|
|UDP|Transport|
|TLS|Presentation (OSI concept) / Application in TCP/IP|
|SSL|Presentation (OSI concept) / Application in TCP/IP|

---

# Reality Check

In modern networking:

- **Session management is often implemented by applications themselves.**
- Many Layer 5 responsibilities are handled by:
    - Web frameworks
    - Databases
    - SSH
    - SMB
    - Operating systems

That's why you'll rarely hear someone say:

> "I'm writing a Session Layer protocol."

Instead, they'll say:

> "I'm implementing session management."