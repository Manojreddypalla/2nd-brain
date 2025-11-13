# **DAILY NOTES — LINUX PROCESS & SYSTEM MONITORING (Day 1)**

_(Covers Units IV–VI Core Concepts and their Practical Use)_

---

## 🧩 1️⃣ `fork()` — Creating Processes (Unit IV)

### 🔹 Concept:

`fork()` is a **system call** that creates a new process (child) by duplicating the parent.

|Process|Return Value of `fork()`|
|---|---|
|Parent|PID of the child (positive integer)|
|Child|0|
|Error|-1|

### 🔹 Key Points:

- After `fork()`, two processes run the same code but with different `pid` values.
- Parent & child execute the next line after `fork()` simultaneously.
- `getpid()` → current process ID
- `getppid()` → parent process ID
- `wait()` → makes parent wait until child finishes

### 🔹 Simple Flow:

```
Parent executes fork()
 ├── Parent branch (pid > 0)
 └── Child branch  (pid == 0)

```

---

## 🧩 2️⃣ `/proc` Filesystem — Kernel Information Source (Foundation of Units V–VI)

### 🔹 Concept:

`/proc` is a **virtual filesystem** that exposes live kernel and system information.

It’s dynamically generated — nothing inside is stored on disk.

### 🔹 Key Files:

|File|Information|Used in Project|
|---|---|---|
|`/proc/stat`|CPU time counters (user, system, idle)|✅ CPU usage|
|`/proc/meminfo`|Memory statistics|✅ Memory usage|
|`/proc/uptime`|System uptime in seconds|✅ Uptime display|
|`/proc/loadavg`|System load & running processes|Optional|
|`/proc/sysvipc/`|Active message queues, semaphores, shared memory|Later (Unit VI)|

### 🔹 Important:

- Every monitoring tool (`top`, `htop`, `free`, `ps`) reads data from `/proc`.
- `/proc/[PID]/` directories hold per-process info (status, cmdline, fd, etc.).

---

## 🧩 3️⃣ `fscanf()` and `%llu` — Reading Data from `/proc`

### 🔹 Usage:

```c
fscanf(fp, "cpu %llu %llu %llu %llu", &user, &nice, &system, &idle);

```

### 🔹 Meaning:

|Token|Purpose|
|---|---|
|`"cpu"`|Literal match for first line of `/proc/stat` (total CPU)|
|`%llu`|Reads **unsigned long long** (very large positive integers)|
|`user, nice, system, idle`|CPU time counters (in jiffies) since boot|

### 🔹 Why `%llu`:

- CPU counters can exceed billions.
- `unsigned long long` (64-bit) avoids overflow.

---

## 🧩 4️⃣ CPU Usage Calculation Formula

From `/proc/stat`:

```
cpu  user  nice  system  idle ...

```

Two readings, 1 sec apart:

```c
usage = (1 - (idle2 - idle1) / (total2 - total1)) * 100;

```

### 🔹 Meaning:

|Term|Description|
|---|---|
|`total2 - total1`|Total CPU time during 1 s|
|`idle2 - idle1`|Idle time during 1 s|
|Result|% of time CPU was busy|

---

## 🧩 5️⃣ Memory Usage Calculation

From `/proc/meminfo`:

```
MemTotal: 16384248 kB
MemFree:  1283948 kB
Buffers:   123456 kB
Cached:   4567890 kB

```

Formula:

```c
used = total - (free + buffers + cached);
usage = (used / total) * 100;

```

---

## 🧩 6️⃣ System Uptime Reading

From `/proc/uptime`:

```
12345.67 6789.12

```

- First number → total uptime (seconds)
- Converted to `HH:MM:SS` for display.

---

## 🧩 7️⃣ The Live CLI Monitor Program (Your Project Start)

### 📁 File: `live_monitor.c`

### 🔹 Functions Used:

|Function|Purpose|
|---|---|
|`fopen()` / `fclose()`|Open/close `/proc` files|
|`fscanf()`|Parse formatted data|
|`sleep(1)`|Wait one second between updates|
|`system("clear")`|Refresh the terminal screen|
|`printf()` / `fflush(stdout)`|Print live results instantly|

### 🔹 Output Example:

```
=====================================
   🧠 Live CLI System Monitor (Mini-htop)
=====================================

CPU Usage   : 12.45%
Memory Usage: 48.72%
Uptime: 01:25:47 (hh:mm:ss)

```

Updates every second → behaves like `htop`.

---

## 🧩 8️⃣ Why “cpu” Appears in `fscanf()`

- `/proc/stat` has many lines (cpu, cpu0, cpu1, intr, etc.).
- Including `"cpu "` ensures you only read the **total** CPU line, not per-core data.

---

## 🧩 9️⃣ Key Takeaways

|Concept|Summary|
|---|---|
|`fork()`|Creates multiple processes (base for IPC)|
|`/proc`|Live kernel data filesystem (used by all monitors)|
|`fscanf()` + `%llu`|Reads large numerical counters safely|
|`cpu`, `nice`, `system`, `idle`|Core CPU usage metrics|
|`sleep()` + `system("clear")`|Creates smooth live terminal updates|
|You built|A minimal **real-time system monitor**|
|Next|Add `fork()`, shared memory & semaphores to make it concurrent (Units V–VI)|

---

## 📘 **Exam Tips**

- `/proc` is a **virtual filesystem** providing process & system stats (not on disk).
- `fork()` → returns child PID in parent, `0` in child.
- `unsigned long long` → safe for CPU tick counters.
- CPU usage = `(busy_time / total_time) × 100`.
- Memory usage = `(used / total) × 100`.
- Real tools like `top`, `ps`, `free` use `/proc`.

---

✅ **You Achieved Today**

- Fully understood how Linux exposes data via `/proc`.
- Wrote a real, working live CLI monitor (`mini-htop`).
- Learned why `fork()`, `%llu`, and `"cpu"` matter.
- Ready to extend into IPC + shared memory in next session.