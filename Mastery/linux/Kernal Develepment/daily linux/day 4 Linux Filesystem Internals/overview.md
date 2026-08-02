## 📖 Theory (10–15 min)

Learn these concepts:

- What is a **filesystem**?
- What is an **inode**?
- What is the difference between:
    - File name
    - File content
    - File metadata
- Hard Link vs Soft (Symbolic) Link

### 🧠 Mental Model

Think of a file like a library book.

```
Filename  ─────► Inode ─────► Actual Data Blocks(report.txt)    (metadata)      (contents)
```

The **filename** is just a label.

The **inode** stores information such as:

- Owner
- Permissions
- Size
- Timestamps
- Location of the data on disk

The **actual file data** is stored separately.

---

## 💻 Hands-on Lab (15–20 min)

Run these commands:

```
pwd
```

See your current working directory.

```
ls -l
```

View permissions and metadata.

```
ls -li
```

Notice the **inode number** in the first column.

```
df -h
```

Check filesystem usage.

```
du -sh .
```

Check the size of the current directory.

---

## 🔬 Mini Experiment (5 min)

Create a file and inspect its inode:

```
touch demo.txtls -li demo.txt
```

Create a **hard link**:

```
ln demo.txt hard_demo.txtls -li demo.txt hard_demo.txt
```

Both files should have the **same inode number**.

Now create a **symbolic link**:

```
ln -s demo.txt soft_demo.txtls -li soft_demo.txt
```

Notice that the symbolic link has **its own inode** and points to the original file.

---

## 📝 Quick Revision (5–10 min)

Write these points:

- A filesystem organizes how data is stored on disk.
- An **inode** stores file metadata, not the filename.
- A **hard link** shares the same inode as the original file.
- A **symbolic link** is a shortcut that points to another file.

---

## 🎯 Today's Goal

By the end of today's session, you should be able to answer:

- What is an inode?
- Why are filenames and inodes different?
- What's the difference between a hard link and a symbolic link?
- What information does `ls -li` reveal?

### ✅ Commands learned today

```
pwdls -lls -lidf -hdu -sh .touchlnln -s
```

Keep the pace steady: **one concept, one lab, one page of notes.** Tomorrow you'll move on to **Users & Permissions**, where you'll learn how Linux controls access to files and system resources.