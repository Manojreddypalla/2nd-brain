# Git & GitHub Interview Notes

---

# Git

## Full Form

**Git** (Not an acronym)

Created by **Linus Torvalds (2005)**

## What is Git?

Git is a **Distributed Version Control System (DVCS)**.

It helps developers:

- Track code changes
    
- Collaborate with others
    
- Restore previous versions
    
- Manage different features using branches
    

Think of Git as a **time machine for your code**.

---

# What is Version Control?

Imagine you have these files:

```text
project_final.cpp
project_final2.cpp
project_final_latest.cpp
project_final_latest_final.cpp
```

😅 This becomes messy.

Git stores every version automatically.

```text
Version 1
      ↓
Version 2
      ↓
Version 3
      ↓
Version 4
```

You can return to any version anytime.

---

# Git vs GitHub

|Git|GitHub|
|---|---|
|Version Control System|Cloud Hosting Platform|
|Installed on your PC|Website|
|Tracks changes|Stores repositories online|
|Works Offline|Requires Internet|

Example

```text
Your Laptop
      │
      │ Git
      ▼
GitHub Repository
```

---

# Repository (Repo)

A repository stores

- Source code
    
- Git history
    
- Branches
    
- Commits
    

Create one

```bash
git init
```

---

# Clone Repository

Downloads an existing repository.

```bash
git clone https://github.com/user/project.git
```

---

# Git Workflow ⭐⭐⭐⭐⭐

```text
Working Directory
        │
git add
        ▼
Staging Area
        │
git commit
        ▼
Local Repository
        │
git push
        ▼
GitHub Repository
```

This is the **most important Git concept**.

---

# Working Directory

Where you edit files.

Example

```text
main.cpp
index.js
App.jsx
```

---

# Staging Area

Temporary area before committing.

Add files

```bash
git add file.txt
```

All files

```bash
git add .
```

---

# Commit

A snapshot of your project.

```bash
git commit -m "Added Login Feature"
```

Every commit has a unique ID called a **SHA Hash**.

---

# Push

Upload commits to GitHub.

```bash
git push origin main
```

---

# Pull

Download latest changes.

```bash
git pull origin main
```

---

# Fetch

Downloads changes without merging.

```bash
git fetch
```

---

# Status

Check current repository status.

```bash
git status
```

Shows

- Modified files
    
- Staged files
    
- Untracked files
    

---

# Log

View commit history.

```bash
git log
```

Compact version

```bash
git log --oneline
```

---

# Branch ⭐⭐⭐⭐⭐

A branch is an independent line of development.

Default branch

```text
main
```

Example

```text
main

│

├── login-feature

├── dashboard

└── payment
```

Develop features without affecting the main branch.

---

# Create Branch

```bash
git branch feature-login
```

---

# List Branches

```bash
git branch
```

---

# Switch Branch

```bash
git checkout feature-login
```

Modern command

```bash
git switch feature-login
```

---

# Create + Switch

```bash
git checkout -b feature-login
```

---

# Merge

Combine branches.

```bash
git checkout main

git merge feature-login
```

---

# Merge Conflict ⭐⭐⭐⭐☆

Occurs when two branches modify the same lines of a file.

Git cannot decide automatically.

You must manually choose the correct code.

---

# Remote

View remote repositories.

```bash
git remote -v
```

---

# Origin

Default name for the remote GitHub repository.

```bash
origin
```

Example

```bash
git push origin main
```

---

# .gitignore ⭐⭐⭐⭐⭐

Tells Git which files to ignore.

Example

```text
node_modules/

.env

dist/

*.log
```

Never upload

- node_modules
    
- .env
    
- build files
    
- logs
    

---

# HEAD

Represents your current commit.

```text
HEAD

↓

Latest Commit
```

---

# Undo Last Commit

Keep changes

```bash
git reset --soft HEAD~1
```

Remove changes

```bash
git reset --hard HEAD~1
```

Use `--hard` carefully because it discards local changes.

---

# Restore File

```bash
git restore file.txt
```

Discard uncommitted changes.

---

# Stash ⭐⭐⭐☆

Temporarily save unfinished work.

```bash
git stash
```

Restore

```bash
git stash pop
```

---

# Pull Request (PR)

A request to merge your branch into another branch.

Typical workflow

```text
Create Branch

↓

Commit

↓

Push

↓

Open Pull Request

↓

Code Review

↓

Merge
```

---

# Code Review

Other developers review your code before merging.

Purpose

- Find bugs
    
- Improve code quality
    
- Maintain coding standards
    

---

# Fork

Creates your own copy of another repository.

Mostly used in open-source projects.

---

# Tags

Used to mark releases.

Example

```text
v1.0

v2.0

v3.0
```

---

# Semantic Versioning

Format

```text
MAJOR.MINOR.PATCH
```

Example

```text
2.4.1
```

Meaning

- Major → Breaking changes
    
- Minor → New features
    
- Patch → Bug fixes
    

---

# Git Branch Workflow

```text
main
 │
 ├── feature-login
 │
 ├── feature-dashboard
 │
 ├── bugfix-payment
 │
 └── feature-profile

↓

Merge

↓

main
```

---

# Common Git Commands

Initialize Repository

```bash
git init
```

Clone

```bash
git clone URL
```

Status

```bash
git status
```

Add Files

```bash
git add .
```

Commit

```bash
git commit -m "message"
```

Push

```bash
git push origin main
```

Pull

```bash
git pull origin main
```

Branches

```bash
git branch
```

Switch Branch

```bash
git checkout branch-name
```

Merge

```bash
git merge branch-name
```

History

```bash
git log --oneline
```

---

# Git Workflow Example

```text
Create Project

↓

git init

↓

Create Files

↓

git add .

↓

git commit -m "Initial Commit"

↓

git remote add origin URL

↓

git push origin main
```

---

# Frequently Asked Interview Questions

### What is Git?

A distributed version control system used to track changes in source code.

---

### What is GitHub?

A cloud platform used to host Git repositories.

---

### Difference between Git and GitHub?

Git manages version history.

GitHub hosts Git repositories online.

---

### What is a Repository?

A folder tracked by Git containing source code and commit history.

---

### What is a Commit?

A snapshot of the project at a specific point in time.

---

### What is Branching?

Creating an independent line of development.

---

### What is Merging?

Combining two branches.

---

### What is a Merge Conflict?

Occurs when Git cannot automatically merge changes.

---

### What is a Pull Request?

A request to merge changes into another branch after review.

---

### What is `.gitignore`?

A file specifying which files Git should ignore.

---

### Why don't we upload `node_modules`?

Because dependencies can be recreated from `package.json` using:

```bash
npm install
```

---

### Why don't we upload `.env`?

It contains secrets like:

- API Keys
    
- Database Passwords
    
- JWT Secrets
    

---

# Full Forms

Git → (Not an acronym)

DVCS → Distributed Version Control System

SHA → Secure Hash Algorithm

PR → Pull Request

---

# Interview Cheat Sheet

|Command|Purpose|
|---|---|
|`git init`|Initialize repository|
|`git clone`|Download repository|
|`git status`|Check changes|
|`git add .`|Stage files|
|`git commit -m`|Save snapshot|
|`git push`|Upload changes|
|`git pull`|Download changes|
|`git fetch`|Download without merging|
|`git branch`|List branches|
|`git checkout`|Switch branch|
|`git merge`|Merge branches|
|`git log --oneline`|View commit history|

---

# Revision Checklist ✅

-  Git
    
-  GitHub
    
-  Version Control
    
-  Repository
    
-  Clone
    
-  Working Directory
    
-  Staging Area
    
-  Commit
    
-  Push
    
-  Pull
    
-  Fetch
    
-  Status
    
-  Log
    
-  Branch
    
-  Merge
    
-  Merge Conflict
    
-  `.gitignore`
    
-  HEAD
    
-  Stash
    
-  Pull Request
    
-  Code Review
    
-  Fork
    
-  Semantic Versioning
    

---

## 🎯 For your internship

If you can confidently explain:

- the Git workflow (`Working Directory → Staging → Commit → Push`),
    
- branching and pull requests,
    
- merge conflicts,
    
- and use the core commands (`init`, `clone`, `add`, `commit`, `push`, `pull`, `branch`, `checkout`, `merge`),
    

you'll meet the Git/GitHub expectations for almost every full-stack internship.

### Remaining major topics from the job description:

1. **Debugging & Browser DevTools**
    
2. **Deployment & Cloud Basics (Vercel, Netlify, Render, AWS basics)**
    
3. **Agile & Code Reviews**
    
4. **Performance Optimization**
    

These are much smaller than React, Express, SQL, and Git, and can each be covered in a concise revision note.