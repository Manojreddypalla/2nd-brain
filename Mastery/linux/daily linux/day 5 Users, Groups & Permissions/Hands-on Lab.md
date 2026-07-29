Run these commands:

```
whoami
```

Who are you logged in as?

```
id
```

View your UID, GID, and groups.

```
groups
```

List all groups you belong to.

```
ls -l
```

Inspect file permissions.

Create a test file:

```
touch linux_lab.txtls -l linux_lab.txt
```

Change permissions:

```
chmod 644 linux_lab.txtls -l linux_lab.txt
```

Make it executable:

```
chmod +x linux_lab.txtls -l linux_lab.txt
```

---

## 🔬 Mini Experiment (5 min)

Create a simple script:

```
echo 'echo "Hello Linux"' > hello.sh
```

Try to run it:

```
./hello.sh
```

If it isn't executable, you'll get **Permission denied**.

Now fix it:

```
chmod +x hello.sh./hello.sh
```

Notice how changing one permission changes what the kernel allows.