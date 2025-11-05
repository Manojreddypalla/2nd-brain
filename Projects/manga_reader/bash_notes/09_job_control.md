# 9. Job Control

Manage processes running in your shell.

-   `command &`: Runs a command in the **background**.
-   `jobs`: Lists all background jobs.
-   `fg %1`: Brings job number 1 to the **foreground**.
-   `Ctrl+Z`: Suspends the current foreground job.
-   `bg %1`: Resumes suspended job number 1 in the **background**.
-   `nohup command &`: "No Hangup". Allows a command to keep running even after you close the terminal. Output is typically redirected to `nohup.out`.

**Example:**

```bash
# Start a long-running process in the background
sleep 100 &

# List background jobs
jobs

# Bring the job to the foreground
fg %1

# Suspend the job with Ctrl+Z

# Resume the job in the background
bg %1

# Use nohup to run a command that will survive terminal closure
nohup sleep 200 &
```
