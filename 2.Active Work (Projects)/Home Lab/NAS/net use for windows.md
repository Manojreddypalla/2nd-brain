# Windows `net use` - Complete Practical Notes

## 🎯 What is `net use`?

`net use` is a Windows command used to:

- Connect to network shared folders (SMB/Samba)
    
- Map them as local drives
    
- Store credentials
    
- Disconnect mapped drives
    
- View current network connections
    

Think of it as telling Windows:

> "Treat this folder on another computer as if it were a local hard drive."

---

# Mental Model

Without `net use`

```text
Windows PC
     │
     ▼
Open Explorer
     │
     ▼
Type \\192.168.1.10\share every time
```

With `net use`

```text
Windows PC
      │
      ▼
Map Network Share
      │
      ▼
Z:
```

Now it behaves like:

```text
C:
D:
E:
Z:
```

---

# Syntax

```cmd
net use [Drive:] \\Server\Share [Password] /user:Username
```

General form:

```cmd
net use Z: \\SERVER\SHARE password /user:user
```

---

# Basic Example

Server

```
100.113.3.11
```

Share

```
ZORO
```

Username

```
honey
```

Password

```
1234
```

Command

```cmd
net use Z: \\100.113.3.11\ZORO 1234 /user:honey
```

Result

```
Windows
    │
    ▼
Drive Z:
```

---

# Another Share

```cmd
net use Y: \\100.113.3.11\SANJI 1234 /user:honey
```

Now Explorer becomes

```
C:
D:
Y: SANJI
Z: ZORO
```

---

# Persistent Mapping

Normally

```cmd
net use Z: \\server\share
```

After reboot

❌ Gone

Use

```cmd
net use Z: \\server\share /persistent:yes
```

Now

```
Boot
 ↓
Automatically reconnect
```

Disable

```cmd
/persistent:no
```

---

# Using Current Logged-in User

If Windows username matches Samba username

```cmd
net use Z: \\100.113.3.11\ZORO
```

No credentials needed.

---

# Specify Username

```cmd
net use Z: \\100.113.3.11\ZORO /user:honey
```

Windows asks password.

---

# Specify Username + Password

```cmd
net use Z: \\100.113.3.11\ZORO 1234 /user:honey
```

Everything supplied.

---

# View Existing Connections

```cmd
net use
```

Example

```
Status      Local
------------------------
OK          Z:
OK          Y:
```

---

# Disconnect One Drive

```cmd
net use Z: /delete
```

or

```cmd
net use Z: /del
```

Result

```
Z:
removed
```

---

# Disconnect Everything

```cmd
net use * /delete
```

Removes every mapped drive.

---

# Map Without Drive Letter

Sometimes you don't want

```
Z:
```

Just create a connection.

```cmd
net use \\100.113.3.11\ZORO
```

---

# Connect Using Hostname

Instead of IP

```cmd
net use Z: \\NAS\Movies
```

Windows resolves

```
NAS
↓

192.168.x.x
```

---

# Different Credentials

```cmd
net use Z: \\server\share password /user:domain\username
```

Example

```cmd
net use Z: \\office\docs pass123 /user:company\john
```

---

# Save Credentials

```cmd
/cached
```

Normally Windows stores credentials in

```
Credential Manager
```

You usually don't need to specify them again.

---

# Display Help

```cmd
net use /?
```

Shows every option.

---

# Common Switches

## `/user`

Specify username.

```cmd
/user:honey
```

---

## `/persistent`

Reconnect after reboot.

```cmd
/persistent:yes
```

---

## `/delete`

Disconnect.

```cmd
net use Z: /delete
```

---

## `*`

Automatically assign a drive letter.

```cmd
net use * \\100.113.3.11\SANJI
```

Windows might choose:

```
Y:
```

or

```
X:
```

---

# Common Errors

## System Error 5

```
Access is denied.
```

Wrong username/password.

---

## System Error 53

```
Network path not found.
```

Cannot reach server.

Check

```
Ping
Firewall
IP
```

---

## System Error 67

```
Network name cannot be found.
```

Share doesn't exist.

Example

```
SANJl
```

instead of

```
SANJI
```

---

## System Error 85

```
Local device already in use.
```

Drive letter already mapped.

Use another drive

```
Y:
```

or remove

```cmd
net use Z: /delete
```

---

## System Error 1219

```
Multiple connections to a server
using more than one username
```

Windows only allows one set of credentials per server.

Remove existing mappings:

```cmd
net use * /delete
```

Then reconnect.

---

# Real Example (Your Setup)

Linux Server

```
100.113.3.11
```

Shares

```
ZORO
SANJI
```

User

```
honey
```

Password

```
1234
```

Commands

```cmd
net use Z: \\100.113.3.11\ZORO 1234 /user:honey /persistent:yes
```

```cmd
net use Y: \\100.113.3.11\SANJI 1234 /user:honey /persistent:yes
```

Verify

```cmd
net use
```

Expected

```
Status    Local    Remote

OK        Z:       \\100.113.3.11\ZORO
OK        Y:       \\100.113.3.11\SANJI
```

---

# Related Commands

|Command|Purpose|
|---|---|
|`net use`|Map/unmap network drives|
|`net view`|List computers and shares on the network|
|`net share`|View or manage shared folders (server side)|
|`net user`|Manage Windows user accounts|
|`net accounts`|Password and account policies|
|`net session`|View active SMB sessions|
|`net file`|View files currently opened over network shares|
|`net statistics`|Show network service statistics|

---

# Quick Cheat Sheet

```cmd
:: Map drive
net use Z: \\server\share

:: Map with credentials
net use Z: \\server\share password /user:username

:: Persistent mapping
net use Z: \\server\share /persistent:yes

:: List mappings
net use

:: Remove one mapping
net use Z: /delete

:: Remove all mappings
net use * /delete

:: Let Windows choose the drive letter
net use * \\server\share

:: Help
net use /?
```

## 💡 Connection to Linux/Samba

When you run:

```cmd
net use Z: \\100.113.3.11\ZORO
```

this is what happens internally:

```text
Windows Explorer
       │
       ▼
net use
       │
       ▼
SMB Client (Windows)
       │
       ▼
Network (TCP/IP, usually port 445)
       │
       ▼
Samba Server (Linux)
       │
       ▼
Shared Directory (/media/manoj/ZORO)
```

So `net use` is simply the Windows-side client command that creates an SMB connection to a Samba (or Windows) file server and optionally exposes it as a drive letter.