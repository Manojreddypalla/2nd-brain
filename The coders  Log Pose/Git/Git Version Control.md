### **Git Version Control — Common Questions and Answers**

---

**Q1: What is Git used for?**

**A:** Git is a **version control system** that helps you track changes in your code, collaborate with others, and revert to previous versions when needed.

---

**Q2: Do I need to add files every time I make a change?**

**A:** ✅ Yes.

Before committing, you must use `git add` to **stage** the files you changed — otherwise Git won’t include those changes in the commit.

---

**Q3: Why do I need to use `git add` before committing?**

**A:** Because Git has three areas:

1. **Working Directory** – where you edit files
2. **Staging Area** – where you mark which changes to include
3. **Repository** – where commits are stored

`git add` moves your changes from the _working directory_ to the _staging area_.

---

**Q4: What happens if I forget to add a file before committing?**

**A:** That file’s changes **won’t be saved** in the new commit.

They’ll remain as “unstaged changes” until you add and commit them later.

---

**Q5: How do I add all changed files at once?**

**A:** Use:

```bash
git add .

```

This adds all modified, new, and deleted files in one go.

---

**Q6: How do I make a commit after adding?**

**A:** Use:

```bash
git commit -m "Your commit message here"

```

This saves a snapshot of all staged files.

---

**Q7: Is there a shortcut to add and commit at once?**

**A:** Yes, use:

```bash
git commit -am "Your message"

```

⚠️ But this only works for **modified** files, not **new** ones.

---

**Q8: What’s a typical Git workflow?**

**A:**

```bash
git add .
git commit -m "Updated project files"
git push origin main

```

1️⃣ Stage changes → 2️⃣ Commit → 3️⃣ Push to remote repo.

---

**Q9: How can I check which files are modified or staged?**

**A:** Use:

```bash
git status

```

It shows which files are:

- Modified (but not staged)
- Staged (ready to commit)
- Untracked (new)

---

Would you like me to add **real terminal examples** (with outputs of `git status` and `git log`) next, so you