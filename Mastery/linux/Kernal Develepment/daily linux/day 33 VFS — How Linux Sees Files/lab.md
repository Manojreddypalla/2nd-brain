# 🧪 Linux Internals — Day 33: VFS Lab

> 🎯 **Goal:** Actually see **inode → hard links → file descriptors → open files → filesystem** instead of just memorizing the VFS diagram.

## Lab 1 — Create the Playground

```bash
mkdir -p /tmp/vfs-lab
cd /tmp/vfs-lab
echo "Linux Internals" > original.txt
```

Check:

```bash
ls -l
```

You should see:

```text
original.txt
```

Our starting point:

```text
original.txt
     ↓
 filesystem object
```

Now we'll dig underneath it.

---

# Lab 2 — Find the Inode ⭐

Run:

```bash
ls -li original.txt
```

`-i` = show **inode number**.

You might see:

```text
183521 -rw-r--r-- 1 retr0 retr0 16 ... original.txt
^^^^^^
inode
```

Then:

```bash
stat original.txt
```

Look for:

```text
Size:
Inode:
Links:
Access:
Modify:
Change:
```

Your mental model:

```text
"original.txt"
      │
      ▼
    dentry
      │
      ▼
inode 183521
      │
      ├── permissions
      ├── owner
      ├── size
      └── timestamps
```

The important thing is already:

> **`original.txt` is a name. The inode represents the underlying filesystem object.**

---

# Lab 3 — Prove `filename ≠ inode` 🔥

Create a hard link:

```bash
ln original.txt second-name.txt
```

Now:

```bash
ls -li
```

You should see something like:

```text
183521 ... original.txt
183521 ... second-name.txt
```

Look carefully.

### Same inode!

```text
original.txt      → 183521
second-name.txt   → 183521
```

Your model is now:

```text
original.txt ────┐
                 │
                 ▼
             inode 183521
                 ▲
                 │
second-name.txt ─┘
```

This is the experiment that proves why **filename and inode must be separate concepts**.

---

# Lab 4 — Modify Through the Other Name

Run:

```bash
echo "Changed through second name" >> second-name.txt
```

Now:

```bash
cat original.txt
```

You'll see:

```text
Linux Internals
Changed through second name
```

But you modified:

```text
second-name.txt
```

Why did `original.txt` change?

Because there aren't two independent file contents.

```text
original.txt ─┐
              ▼
            inode
              ▲
second.txt ───┘
              │
              ▼
         underlying data
```

Both names lead to the **same inode/object**.

---

# Lab 5 — Check Link Count

Run:

```bash
stat original.txt
```

Look for:

```text
Links: 2
```

Why `2`?

Because:

```text
             inode
            /     \
           /       \
original.txt     second-name.txt

2 hard links
```

Now remove one name:

```bash
rm second-name.txt
```

Check:

```bash
stat original.txt
```

Now:

```text
Links: 1
```

Important insight:

```text
rm filename
```

doesn't conceptually mean:

> "Immediately destroy the inode and data."

It removes a directory entry/link.

Very simplified:

```text
rm second-name.txt

second-name.txt ─X─→ inode

original.txt ──────→ inode
```

The object is still reachable through `original.txt`.

---

# Lab 6 — Which Filesystem Is Underneath?

Run:

```bash
df -T /tmp/vfs-lab
```

`-T` = show filesystem **type**.

You might see:

```text
Filesystem       Type
/dev/nvme0n1p... ext4
```

or perhaps another filesystem depending on your setup.

Now connect it:

```text
original.txt
     ↓
VFS
     ↓
ext4 / XFS / tmpfs / ...
     ↓
actual backing implementation
```

Your shell didn't need to know which filesystem was underneath when you ran:

```bash
cat original.txt
```

That's exactly the problem VFS solves.

---

# Lab 7 — Open the File Manually With an FD ⭐

This is where your previous file-descriptor lessons connect to VFS.

Run:

```bash
exec 3<original.txt
```

This tells the shell to open `original.txt` for input on:

```text
FD 3
```

Now:

```bash
ls -l /proc/$$/fd
```

Find:

```text
3 -> /tmp/vfs-lab/original.txt
```

You now have:

```text
Shell Process
      │
      ▼
FD Table
      │
      ├── 0 → terminal
      ├── 1 → terminal
      ├── 2 → terminal
      │
      └── 3 → original.txt
```

But internally, think deeper:

```text
Process
   ↓
FD 3
   ↓
struct file
   ↓
path/dentry
   ↓
inode
   ↓
Filesystem
```

---

# Lab 8 — Actually Read Using FD 3

You don't even need to say `original.txt` anymore.

Run:

```bash
cat <&3
```

You're saying:

> Read using **FD 3**.

You should get your file contents.

Why?

Because the filename was needed during opening/path lookup.

Once the file is open:

```text
FD 3
 ↓
open-file object
 ↓
filesystem object
```

The process can operate through the descriptor.

That's a very important systems concept.

---

# Lab 9 — See Open-File Position ⭐

Close FD 3 first:

```bash
exec 3<&-
```

Reopen it:

```bash
exec 3<original.txt
```

Read one line:

```bash
read -u 3 line
```

Print it:

```bash
echo "$line"
```

Now read again:

```bash
read -u 3 line
echo "$line"
```

Notice the second read continues from where the first one stopped.

Why?

Because the open-file instance maintains state such as the **current file offset**.

Conceptually:

```text
FD 3
 ↓
struct file
 │
 ├── current position
 ├── flags
 └── file reference
       ↓
      inode
```

This is one reason Linux needs something like `struct file` in addition to an inode.

---

# Lab 10 — Two Opens, Same Inode, Different Positions 🔥

This is the experiment that makes `struct file` click.

First make a multi-line file:

```bash
printf "ONE\nTWO\nTHREE\nFOUR\n" > numbers.txt
```

Open it twice:

```bash
exec 3<numbers.txt
exec 4<numbers.txt
```

Now you have:

```text
Process
  │
  ├── FD 3 → open instance A
  │
  └── FD 4 → open instance B
                  │
          same underlying file
```

Read from FD 3:

```bash
read -u 3 line
echo "$line"
```

Output:

```text
ONE
```

Read FD 3 again:

```bash
read -u 3 line
echo "$line"
```

Output:

```text
TWO
```

So FD 3 is around:

```text
ONE
TWO
--- position ---
THREE
FOUR
```

Now read **FD 4**:

```bash
read -u 4 line
echo "$line"
```

What should happen?

You should get:

```text
ONE
```

Not `THREE`.

🔥 Why?

Because these are separate opens:

```text
               numbers.txt
                    │
                   inode
                  ▲     ▲
                 /       \
        struct file A   struct file B
        position=after   position=after
        TWO              ONE
             ▲               ▲
             │               │
            FD 3            FD 4
```

Same underlying filesystem object.

Different **open-file state**.

That's the reason this distinction exists.

---

# Lab 11 — Inspect Those FDs

While FD 3 and FD 4 are open:

```bash
ls -l /proc/$$/fd
```

You'll see:

```text
3 -> /tmp/vfs-lab/numbers.txt
4 -> /tmp/vfs-lab/numbers.txt
```

Two descriptors:

```text
FD 3 ──┐
       ├──→ same pathname/file
FD 4 ──┘
```

But independent open states because we opened the file twice.

You can even inspect descriptor info:

```bash
cat /proc/$$/fdinfo/3
```

and:

```bash
cat /proc/$$/fdinfo/4
```

Look for:

```text
pos:
flags:
```

`pos` is especially interesting.

Read another line from FD 3:

```bash
read -u 3 line
```

Then:

```bash
cat /proc/$$/fdinfo/3
cat /proc/$$/fdinfo/4
```

You can literally observe their positions diverging.

That's a fantastic little kernel experiment.

---

# Lab 12 — Connect With `lsof`

Run:

```bash
lsof -p $$
```

Look for:

```text
numbers.txt
```

Now you have three different views of the same idea:

```text
/proc/$$/fd
     ↓
Raw FD information


lsof -p $$
     ↓
Human-friendly open resources


VFS model
     ↓
FD → struct file → inode
```

---

# Lab 13 — Connect With `strace`

Close your descriptors:

```bash
exec 3<&-
exec 4<&-
```

Now run:

```bash
strace -e trace=openat,read,write,close cat original.txt
```

Look for the story:

```text
openat(... "original.txt" ...) = FD
            ↓
          read(FD)
            ↓
          write(1)
            ↓
          close(FD)
```

Now connect all three tools:

```text
strace
   ↓
See the open/read/close operations


lsof
   ↓
See currently open resources


/proc/PID/fd
   ↓
See process descriptors


VFS
   ↓
Kernel abstraction underneath it all
```

---

# 🔥 Final Experiment — Delete an Open File

This one is cool.

Create:

```bash
echo "I am still here" > ghost.txt
```

Open it:

```bash
exec 3<ghost.txt
```

Now delete the filename:

```bash
rm ghost.txt
```

Check:

```bash
ls
```

`ghost.txt` is gone.

But now:

```bash
ls -l /proc/$$/fd/3
```

You may see something like:

```text
/proc/.../fd/3 -> /tmp/vfs-lab/ghost.txt (deleted)
```

Now try:

```bash
cat <&3
```

🔥 The contents can still be read.

Why?

Because:

```text
Before rm:

ghost.txt
    ↓
dentry/name
    ↓
inode
    ↑
struct file
    ↑
FD 3


After rm:

ghost.txt ❌

FD 3
 ↓
struct file
 ↓
inode
 ↓
data STILL exists
```

The pathname/link was removed, but the process still holds the file open.

Close it:

```bash
exec 3<&-
```

Once there are no directory links and no open references, the filesystem can finally reclaim the file's storage.

This also explains the classic Linux problem:

```text
"I deleted a giant log file...

WHY IS MY DISK STILL FULL?!"
```

A process may still have that deleted file open.

---

# 🧹 Cleanup

When you're done:

```bash
cd /
rm -rf /tmp/vfs-lab
```

---

# 🧠 What You Should Leave the Lab Understanding

Don't memorize commands. You should now be able to visualize:

```text
                 PROCESS
                    │
             File Descriptor
                    │
                    ▼
               struct file
                    │
           open-file state
           position / flags
                    │
                    ▼
               path/dentry
                    │
                    ▼
                  inode
                    │
                    ▼
             filesystem/VFS
```

And today's three big experiments proved three different things:

```text
Hard link experiment
        ↓
filename ≠ inode


FD 3 + FD 4 experiment
        ↓
open instance ≠ inode


Delete-open-file experiment
        ↓
removing a pathname doesn't necessarily
destroy an object that's still referenced
```

If those **three things** make sense, you've understood the important part of Day 33 rather than just memorizing four VFS terms.