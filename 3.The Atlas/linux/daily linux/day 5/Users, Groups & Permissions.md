# 🎯 Core Intuition

Linux is a **multi-user operating system**.

That means hundreds (or even thousands) of users can use the same machine.

So Linux must answer one question every time someone tries to access a file:

> **"Who are you, and are you allowed to do this?"**

The Linux **kernel** checks this before allowing any operation.

Think of Linux like a company.

```
Company (Linux)│├── Employees (Users)├── Teams (Groups)├── Documents (Files)└── Security Guard (Kernel)
```

Every time someone opens a document...

```
Person   │   ▼Kernel checks identity   │   ▼Permission granted?   │ ├── Yes → Access └── No  → Permission Denied
```

---

# 1. What is a User?

A **User** is simply an identity in Linux.

It represents **someone or something** using the computer.

Examples

```
manojubunturootwww-datamysqlpostgres
```

Notice something interesting:

Not every user is a human.

Many programs also run as users.

For example

```
Web Server    │Runs as    ▼www-data
```

This prevents programs from accessing everything.

---

## Why?

Imagine Chrome ran as Administrator all the time.

A browser exploit could destroy your entire computer.

Instead,

```
Chrome   │Runs with limited permissions
```

Linux follows the same idea.

---

# 2. What is a Group?

A **Group** is a collection of users.

Instead of giving permissions one user at a time...

Linux allows you to assign permissions to a whole group.

Example

```
Developers│├── Alice├── Bob├── Charlie
```

Now if a project folder belongs to

```
Group = Developers
```

Everyone inside that group automatically gets access.

---

Think of it like a WhatsApp group.

Instead of messaging everyone individually...

You message the group once.

Linux does exactly that.

---

# 3. UID (User ID)

Every user has a unique number.

That number is called the

> **UID = User Identifier**

Example

```
Name      UIDroot       0manoj   1000alice   1001
```

Although you see names...

The kernel actually uses numbers.

Internally

```
manoj↓UID = 1000↓Kernel stores 1000
```

Numbers are faster to compare than strings.

---

### Important

```
UID 0 = root
```

UID 0 is special.

It bypasses almost every permission check.

---

# 4. GID (Group ID)

Exactly like UID...

Groups also have IDs.

Example

```
developers → GID 1002students → GID 1003
```

Internally Linux stores

```
Owner UIDOwner GID
```

instead of names.

---

# 5. Every File Has an Identity

Whenever you create a file

```
touch notes.txt
```

Linux automatically stores

```
notes.txtOwner = manojGroup = manojPermissions = rw-r--r--
```

Every file has metadata like

```
File├── Owner UID├── Group GID├── Permissions├── Size├── Creation Time├── Modification Time└── Inode Number
```

This information is stored in the file's **inode** (metadata structure).

---

# 6. File Owner vs Group vs Others

Every file divides the world into only **three categories**.

```
          File            │     ┌──────┼──────┐     │      │      │ Owner   Group   Others
```

Linux checks permissions in this order.

---

## Owner

The person who created the file.

Example

```
manoj
```

---

## Group

Everyone belonging to the file's group.

Example

```
developers
```

---

## Others

Literally everyone else.

```
Every remaining user
```

---

Visual

```
notes.txtOwner → manojGroup → developersOthers → everyone else
```

---

# 7. What does rwx mean?

Each file has **three permissions**.

```
rwx
```

---

## r = Read

Allows viewing the contents.

Examples

```
catlessmorenano
```

Need **Read** permission.

---

## w = Write

Allows modifying the file.

Examples

```
echonanovimtruncate
```

Need **Write** permission.

---

## x = Execute

Allows running the file as a program or script.

Example

```
./script.sh
```

Without execute permission

```
Permission denied
```

---

# Permission Layout

Example

```
-rwxr-xr--
```

Break it into chunks.

```
rwx | r-x | r--Owner Group Others
```

Meaning

Owner

```
ReadWriteExecute
```

Group

```
ReadExecute
```

Others

```
Read only
```

---

Memory trick

```
First 3 letters↓OwnerNext 3↓GroupLast 3↓Others
```

---

# Example

```
-rw-r--r--
```

Split

```
rw- | r-- | r--
```

Meaning

Owner

```
ReadWrite
```

Group

```
Read
```

Others

```
Read
```

Nobody can execute it.

---

# Another Example

```
-rwx------
```

Split

```
rwx | --- | ---
```

Only the owner can do anything.

Everyone else gets nothing.

---

# 8. Why does sudo exist?

Suppose you're a normal user.

```
manoj
```

You try

```
Install softwareDelete system filesChange network settings
```

Linux blocks it.

Why?

Because normal users shouldn't accidentally damage the operating system.

---

## The Root User

Linux has a superuser called

```
root
```

Root can do almost anything.

```
Delete any fileKill any processInstall softwareCreate usersShutdown systemModify kernel settings
```

This is powerful—and dangerous.

---

## Why Not Always Log in as Root?

Imagine one typo:

```
rm -rf /
```

As root...

That command could wipe almost the entire filesystem.

As a normal user, many destructive operations are blocked by permissions.

---

## The Role of `sudo`

Instead of logging in as root all the time, Linux lets trusted users **temporarily borrow root privileges** for a single command.

```
You │ ▼sudo │ ▼Kernel │ ▼Runs that command as root
```

This follows the **Principle of Least Privilege**: use elevated permissions only when necessary, reducing the chance of accidental or malicious system-wide changes.

---

# 🧠 Mental Model

Every file has an identity card.

```
File│├── Owner (User)├── Group└── Permissions      │      ├── Owner → rwx      ├── Group → r-x      └── Others → r--
```

Whenever a process tries to access a file, the kernel asks:

1. **Who is making the request?** (UID)
2. **Does that UID own the file?**
3. **If not, is the UID a member of the file's group (GID)?**
4. **If neither, treat it as "Others".**
5. **Check the required permission (r, w, or x).**
6. **Allow or deny the operation.**

```
Process requests access        │        ▼Kernel checks UID/GID        │        ▼Owner?   │ Yes ──► Check Owner permissions   │ No   ▼In Group?   │ Yes ──► Check Group permissions   │ No   ▼Check Others permissions   │   ▼Allow or Permission Denied
```

---

# 🔑 Key Takeaways

- **User** = An identity (person or service) on the system.
- **Group** = A collection of users for easier permission management.
- **UID** = Numeric User ID used internally by the kernel.
- **GID** = Numeric Group ID.
- Every file stores an **Owner**, a **Group**, and **Permissions** in its inode.
- Permissions are divided into **Owner**, **Group**, and **Others**.
- `r` = Read, `w` = Write, `x` = Execute.
- `root` (UID 0) is the superuser with almost unrestricted access.
- `sudo` lets authorized users temporarily execute a command with root privileges instead of working as root all the time.