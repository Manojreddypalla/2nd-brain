# 🐧 Linux Internals — Day 27: I/O Multiplexing

## 1. First: What Problem Are We Solving?

Imagine you're building a web server.

```text
Client 1 ──┐
Client 2 ──┤
Client 3 ──┤
...        ├──→ Server
Client 10000┘
```

Each client has a **TCP socket**.

In Linux, each socket is represented by a **file descriptor (FD)**.

```text
Server Process

fd 3  → Client 1 socket
fd 4  → Client 2 socket
fd 5  → Client 3 socket
...
fd 10002 → Client 10000 socket
```

Now the server needs to know:

> **Which socket currently has data ready for me to read/write?**

That's the core problem.

---

# 2. The Naive Solution — One Thread Per Client

We could create one thread for every connection.

```text
Client 1  → Thread 1
Client 2  → Thread 2
Client 3  → Thread 3
...
Client 10000 → Thread 10000
```

Each thread could simply block:

```text
read(socket)
```

until its client sends something.

### Problem

10,000 threads means lots of:

- stack memory
    
- scheduling work
    
- context switching
    
- kernel bookkeeping
    

Many clients may also be doing absolutely nothing.

```text
10,000 clients

9,990 → 😴 idle
   10 → actually sending data
```

It would be nice if **one/few threads could efficiently manage many sockets**.

That's where **I/O multiplexing** comes in.

---

# 3. What is I/O Multiplexing?

I/O multiplexing means:

> **One thread waits for activity on many file descriptors.**

Instead of:

```text
Thread 1 → FD 3
Thread 2 → FD 4
Thread 3 → FD 5
```

we can have:

```text
             Thread
                │
                ▼
        I/O Multiplexer
         /     |     \
        /      |      \
      FD3     FD4     FD5
```

Linux provides three famous APIs:

```text
select()
poll()
epoll()
```

---

# 4. `select()`

With `select()`, the application gives the kernel sets of FDs and asks:

> **Which of these are ready?**

Conceptually:

```text
Application

FD 3
FD 4
FD 5
FD 6
FD 7
  │
  ▼
select()
  │
  ▼
Kernel
  │
  ▼
Ready: FD 5
```

Then the application handles FD 5.

### Problem

As the number of FDs increases, the application/kernel must repeatedly process the descriptor sets.

```text
10,000 FDs

check/process FD 1
check/process FD 2
check/process FD 3
...
check/process FD 10000
```

Also, traditional `select()` has an `FD_SETSIZE` limitation, commonly 1024.

### Mental model

```text
select()

"Here are all my students.
Tell me who raised their hand."
```

Repeated every time.

---

# 5. `poll()`

`poll()` was designed as a more flexible alternative.

Conceptually:

```text
Application
    │
    │ array of FDs
    ▼
poll()
    │
    ▼
Kernel
```

Unlike `select()`, it doesn't use the same fixed-size FD bitset interface.

So handling large-numbered FDs is easier.

But the fundamental scalability issue remains:

> The FD list still has to be examined/processed each time.

```text
poll()

FD 1   ❌
FD 2   ❌
FD 3   ❌
FD 4   ✅
FD 5   ❌
...
```

---

# 6. `epoll()`

`epoll` changes the model.

Instead of repeatedly passing your entire FD collection:

```text
"Check these 10,000 FDs."
```

you first **register the FDs you're interested in with an epoll instance**.

Then you wait:

```text
epoll_wait()
```

When events are ready, you receive a list of the **ready FDs**.

Conceptually:

```text
10,000 registered sockets

        Kernel
          │
    tracks readiness
          │
          ▼
      Ready List
    ┌────────────┐
    │ FD 18      │
    │ FD 451     │
    │ FD 9001    │
    └────────────┘
          │
          ▼
     epoll_wait()
          │
          ▼
     Application
```

Your program handles those ready sockets.

---

# 7. `select()` vs `epoll()`

Imagine:

```text
10,000 connections
```

but only:

```text
3 have activity.
```

### `select()` mental model

```text
Here are my 10,000 FDs
        ↓
process descriptor sets
        ↓
FD 1       ❌
FD 2       ❌
FD 3       ❌
...
FD 500     ✅
...
FD 5000    ✅
...
FD 9000    ✅
```

### `epoll`

The FDs were registered beforehand.

```text
Kernel tracks them
       ↓
Activity occurs
       ↓
Ready events
┌─────────────┐
│ FD 500      │
│ FD 5000     │
│ FD 9000     │
└─────────────┘
       ↓
Application
```

That's the intuition behind why `epoll` performs well with large numbers of mostly-idle connections.

---

# 8. How `epoll` Works

There are three important operations.

### Step 1 — Create epoll instance

```c
epoll_create1(...)
```

Conceptually:

```text
Application
     │
     ▼
Kernel creates
"epoll instance"
```

Think of it as:

> **My collection/watch mechanism for FDs.**

---

### Step 2 — Register FDs

Using:

```c
epoll_ctl()
```

Conceptually:

```text
epoll instance

watch FD 4
watch FD 5
watch FD 8
watch FD 12
...
```

You can add, modify, or remove watched descriptors.

---

### Step 3 — Wait

```c
epoll_wait()
```

Your thread can sleep until events become available.

```text
Application
     │
     ▼
epoll_wait()
     │
     │ sleep/block
     ▼

     Kernel

        ...

socket receives data
        ↓
FD becomes ready
        ↓
epoll_wait() returns
        ↓
Application handles FD
```

This is the important part.

The CPU doesn't need your application to spin constantly doing:

```text
ready?
ready?
ready?
ready?
ready?
```

The thread can block until something useful happens.

---

# 9. The Event Loop

This leads to one of the most important patterns in server programming:

## **Event Loop**

Conceptually:

```text
while(server_running) {

    events = epoll_wait();

    for(each ready event) {

        handle(event);

    }
}
```

Mental model:

```text
          ┌──────────────┐
          │ epoll_wait() │
          └──────┬───────┘
                 │
            events ready
                 ↓
          Handle events
                 │
                 ▼
          ┌──────────────┐
          │ epoll_wait() │
          └──────────────┘
```

One/few threads can therefore coordinate huge numbers of connections.

---

# 10. Why File Descriptors Matter

You've already learned Linux uses file descriptors as handles for many I/O resources.

```text
Process FD Table

0 → stdin
1 → stdout
2 → stderr

3 → file
4 → TCP socket
5 → pipe
6 → UNIX socket
...
```

That's why the same I/O multiplexing idea isn't restricted only to TCP networking.

`epoll` operates on compatible **file descriptors**.

This connects directly to your previous IPC/networking lessons.

---

# 11. Blocking vs I/O Multiplexing

### Traditional blocking read

```text
read(socket)
     │
     ▼
No data
     │
     ▼
Thread sleeps
     │
     ▼
data arrives
     │
     ▼
read continues
```

Fine for one socket.

But with thousands of sockets, assigning a dedicated blocked thread to each can become expensive.

### Multiplexing

```text
Socket 1 ─┐
Socket 2 ─┤
Socket 3 ─┤
...       ├──→ epoll_wait()
Socket N ─┘         │
                    ▼
               Thread sleeps
                    │
               event occurs
                    ▼
                wake up
                    │
                    ▼
              Handle ready FD
```

That's the fundamental idea.

---

# 12. `select` vs `poll` vs `epoll`

|Feature|`select()`|`poll()`|`epoll()`|
|---|---|---|---|
|Multiple FDs|✅|✅|✅|
|Traditional fixed FD-set limit|Yes|No|No|
|Re-submit/process whole watch set|Yes|Yes|No|
|Returns readiness info|Yes|Yes|Yes|
|Designed to scale to many FDs|Poorer|Poorer|Better|
|Linux-specific|No|No|Yes|

Don't memorize implementation complexity like "`epoll` is always O(1)." Real performance is more nuanced.

The important intuition is:

```text
select/poll
     ↓
Repeatedly provide/process
the whole interest set

epoll
     ↓
Register interest once
     +
receive ready events
```

---

# 13. Level-Triggered vs Edge-Triggered

`epoll` supports two important styles.

### Level Triggered

Default behavior.

Think:

> **“Keep telling me while data is still available.”**

```text
Data arrives
   ↓
READY
   ↓
You read some

data still remains
   ↓
READY again
```

Easier to work with.

---

### Edge Triggered

Using `EPOLLET`.

Think:

> **“Tell me when the state changes.”**

```text
No data
   │
   │ data arrives
   ▼
READY ← notification

You should drain available data
```

Edge-triggered programming commonly goes together with **non-blocking I/O** and reading/writing until you'd block (`EAGAIN`/`EWOULDBLOCK`).

It's more efficient in some designs but easier to get wrong.

For now remember:

```text
Level → remind me while ready

Edge → notify me about readiness transition
```

---

# 14. Why High-Performance Servers Use This Pattern

Servers such as [Nginx](https://nginx.org/?utm_source=chatgpt.com) are designed around event-driven architectures on supported platforms.

Imagine:

```text
               SERVER

Client 1 ──┐
Client 2 ──┤
Client 3 ──┤
...        ├──→ sockets
Client N ──┘       │
                   ▼
                 epoll
                   │
              ready events
                   │
                   ▼
               Event Loop
                   │
                   ▼
            Handle requests
```

Instead of requiring:

```text
10,000 clients
      ↓
10,000 dedicated threads
```

you can build architectures where a smaller number of event-loop workers handle many concurrent sockets.

---

# 🔗 Connect Everything You've Learned

This topic is basically several earlier Linux concepts joining together:

```text
                  PROCESS
                     │
                 FD Table
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
      Files        Pipes        Sockets
                                  │
                                  ▼
                           Thousands of clients
                                  │
                                  ▼
                              epoll()
                                  │
                                  ▼
                            Ready events
                                  │
                                  ▼
                            Application
```

And `epoll` itself is exposed through kernel system-call interfaces:

```text
Application
     │
     ├── epoll_create1()
     │
     ├── epoll_ctl()
     │
     └── epoll_wait()
             │
             ▼
        Linux Kernel
```

---

# 📝 Short Revision Notes

**I/O Multiplexing** → one thread waits for events from many FDs.

**`select()`** → older portable mechanism; repeatedly works with descriptor sets and has traditional FD-set limitations.

**`poll()`** → more flexible FD-array interface, but still processes the supplied FD collection each call.

**`epoll()`** → Linux mechanism where FDs are registered with an epoll instance and ready events are returned to the application.

**`epoll_create1()`** → create epoll instance.

**`epoll_ctl()`** → add/modify/remove watched FDs.

**`epoll_wait()`** → wait for ready events.

**Level-triggered** → reports readiness while the condition remains.

**Edge-triggered** → reports readiness transitions; commonly paired with non-blocking I/O.

## 🧠 The One Diagram to Remember

```text
       10,000 Clients
             │
             ▼
       10,000 Sockets
             │
             ▼
       File Descriptors
             │
             ▼
           epoll
             │
       "Who is ready?"
             │
             ▼
      FD 18, 451, 9001
             │
             ▼
         Event Loop
             │
             ▼
      Handle those clients
```

### ⭐ One-line takeaway

> **`epoll` lets a Linux process efficiently wait for activity across many file descriptors, making event-driven handling of thousands of mostly-idle network connections practical.**