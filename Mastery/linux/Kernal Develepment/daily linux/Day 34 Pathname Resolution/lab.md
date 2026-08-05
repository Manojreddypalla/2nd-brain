# 🧪 Linux Day 34 — Pathname Resolution Lab

> 🎯 **Goal:** See how Linux walks `/ → directory → directory → file`, handles permissions, and follows symlinks.

## 1. Create the Lab

```bash
mkdir -p /tmp/path-lab/a/b/c
echo "kernel journey" > /tmp/path-lab/a/b/c/file.txt
```

Structure:

```text
/
└── tmp
    └── path-lab
        └── a
            └── b
                └── c
                    └── file.txt
```

**Note:** A pathname is a **route**, not the identity of the file.

---

## 2. Watch Path Traversal ⭐

```bash
namei /tmp/path-lab/a/b/c/file.txt
```

You'll get something similar to:

```text
f: /tmp/path-lab/a/b/c/file.txt
 d /
 d tmp
 d path-lab
 d a
 d b
 d c
 - file.txt
```

Linux conceptually walks:

```text
/
↓
tmp
↓
path-lab
↓
a
↓
b
↓
c
↓
file.txt
```

**Note:** Each component must be resolved before Linux can reach the next one.

---

## 3. Check Permissions During the Walk

```bash
namei -l /tmp/path-lab/a/b/c/file.txt
```

`-l` = long listing.

Look at the `x` permission on directories.

```text
drwxr-xr-x /
drwxrwxrwt tmp
drwxr-xr-x path-lab
...
-rw-r--r-- file.txt
```

### Key idea

For directories:

```text
x → search/traverse
```

Therefore:

```text
/
↓ ✅
tmp
↓ ✅
a
↓ ❌ no x permission
STOP
```

Even if `file.txt` itself is readable, you can't reach it if a directory in the path blocks traversal.

---

## 4. Absolute vs Relative Path

Go inside:

```bash
cd /tmp/path-lab
```

Absolute:

```bash
cat /tmp/path-lab/a/b/c/file.txt
```

Relative:

```bash
cat a/b/c/file.txt
```

Both reach the same file, but start differently:

```text
Absolute
   ↓
start from /


Relative
   ↓
start from CWD
```

Check your shell's CWD:

```bash
readlink /proc/$$/cwd
```

You should see:

```text
/tmp/path-lab
```

**Note:** Every process has a current working directory used for relative path resolution.

---

## 5. Test `.` and `..`

```bash
cd /tmp/path-lab/a/b
pwd
```

Then:

```bash
cd ./c
pwd
```

`.` means:

```text
current directory
```

Now:

```bash
cd ..
pwd
```

`..` means:

```text
parent
```

Mental model:

```text
a
└── b ← ..
    └── c ← .
```

---

# 6. Symlink Experiment ⭐

Create:

```bash
ln -s /tmp/path-lab/a/b/c/file.txt /tmp/my-link
```

Inspect:

```bash
ls -l /tmp/my-link
```

Then:

```bash
readlink /tmp/my-link
```

You'll see:

```text
/tmp/path-lab/a/b/c/file.txt
```

Now:

```bash
cat /tmp/my-link
```

Output:

```text
kernel journey
```

What happened?

```text
/tmp/my-link
     ↓
symlink
     ↓
contains another path
     ↓
/tmp/path-lab/a/b/c/file.txt
     ↓
pathname resolution continues
     ↓
file
```

### Short note

```text
Hard link → another name for same inode

Symlink → separate object containing another path
```

---

# 7. Break the Symlink

Rename the target:

```bash
mv /tmp/path-lab/a/b/c/file.txt /tmp/path-lab/a/b/c/new.txt
```

Now:

```bash
cat /tmp/my-link
```

You'll get something like:

```text
No such file or directory
```

But:

```bash
ls -l /tmp/my-link
```

the symlink itself still exists.

Why?

```text
my-link
   ↓
points to path
   ↓
.../file.txt
   ↓
❌ target no longer exists
```

That's a **dangling/broken symlink**.

Restore it:

```bash
mv /tmp/path-lab/a/b/c/new.txt /tmp/path-lab/a/b/c/file.txt
```

---

# 8. Watch the System Call

Run:

```bash
strace -e trace=%file cat /tmp/path-lab/a/b/c/file.txt
```

Don't worry about all the output.

Look for something like:

```text
openat(... "/tmp/path-lab/a/b/c/file.txt" ...)
```

Mental model:

```text
cat
 │
 │ openat()
 ▼
Kernel
 │
 ▼
VFS
 │
 ▼
Pathname Resolution
 │
 ├─ /
 ├─ tmp
 ├─ path-lab
 ├─ a
 ├─ b
 ├─ c
 └─ file.txt
      ↓
 final inode
```

`strace` shows the **system-call request**; the component-by-component VFS walk happens inside the kernel.

---

# 9. Connect It to Day 33 🔥

Yesterday:

```text
Process
 ↓
FD
 ↓
struct file
 ↓
dentry/inode
```

Today's missing part:

```text
PATH
 ↓
pathname resolution
 ↓
dentry/inode
```

Together:

```text
"/tmp/path-lab/a/b/c/file.txt"
              ↓
       Path Resolution
              ↓
        dentry/inode
              ↓
         struct file
              ↓
             FD
              ↓
           Process
```

That's the whole point of today's lab.

---

## 🧹 Cleanup

```bash
rm -f /tmp/my-link
rm -rf /tmp/path-lab
```

# 📝 Day 34 Short Notes

```text
Pathname Resolution
→ Linux walks paths component-by-component.

Absolute path
→ starts from /

Relative path
→ starts from process CWD

.
→ current directory

..
→ parent

dentry
→ represents/caches pathname components for VFS lookup

dcache
→ caches dentries → faster repeated path lookup

Directory x permission
→ allows search/traversal

Hard link
→ another name referencing same inode

Symlink
→ separate object containing another pathname

Path:
/
↓
home
↓
retr0
↓
file
↓
dentry/inode

Useful:
namei PATH
namei -l PATH
readlink PATH
readlink /proc/$$/cwd
strace -e trace=%file command
```

### ⭐ Remember just this

```text
PATH
 ↓
Walk each component
 ↓
Check directories + permissions
 ↓
Handle symlinks/mounts
 ↓
Find final dentry/inode
 ↓
Open file
 ↓
struct file
 ↓
FD
```

If you can visualize that chain, **Day 34 is done**.