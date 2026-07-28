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