# branch switching

ou can change branches in Git using the `git checkout` command.

---

### Switch to an Existing Branch

To switch to a branch that already exists, use the following command. This will update the files in your working directory to match the version stored in that branch.

Bash

`git checkout branch-name`

For example, to switch to a branch called `develop`, you would run:

Bash

`git checkout develop`

---

### Create and Switch to a New Branch

If you want to create a new branch and switch to it in a single step, use the `-b` flag with `git checkout`.

Bash

`git checkout -b new-branch-name`

For instance, to create a new branch called `feature/new-login` and switch to it immediately, you would use:

Bash

`git checkout -b feature/new-login`

---

### A Quick Tip 💡

If you're not sure what branches are available in your local repository, you can list them all with this command:

Bash

`git branch`

The branch you are currently on will be marked with an asterisk (*).