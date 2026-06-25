# 1. Socket Programming (Most Important)

Without sockets, nothing else matters.

Understand:

```
socket()connect()send()recv()close()
```

Mental model:

```
Your Program      |   Socket      | Operating System      |   Network      | Target Machine
```

A socket is just a file descriptor that represents a network connection.

Learn:

```
sockaddr_inAF_INETSOCK_STREAMhtons()inet_pton()
```

---

# 2. TCP/IP Basics

You don't need a network engineer's knowledge.

Just know:

```
IP = House AddressPort = Room NumberTCP = Reliable Delivery Service
```

Understand:

```
SYNSYN-ACKACK
```

and why:

```
connect()
```

succeeds or fails.

---

# 3. Threads

For a scanner, threads are where speed comes from.

Without threads:

```
Port 1waitPort 2waitPort 3wait
```

With threads:

```
Thread A -> ports 1-100Thread B -> ports 101-200Thread C -> ports 201-300
```

Learn:

```
std::threadjoin()
```

Then:

```
std::mutexstd::lock_guard
```

because multiple threads will print results simultaneously.

Example:

```
std::mutex m;void scan(int port){    if(open)    {        std::lock_guard<std::mutex> lock(m);        std::cout << port << "\n";    }}
```

---

# 4. Service Detection

This is where things get interesting.

Finding:

```
80 OPEN
```

is easy.

Finding:

```
80 -> Apache443 -> Nginx22 -> OpenSSH
```

requires talking to the service.

For example:

## HTTP

Send:

```
GET / HTTP/1.1Host: example.com
```

Receive:

```
Server: nginx
```

---

## SSH

SSH often sends a banner immediately:

```
SSH-2.0-OpenSSH_9.6
```

You simply read it.

---

## FTP

Port:

```
21
```

Banner:

```
220 FTP Server Ready
```

---

Concepts needed:

```
send()recv()char buffer[4096]
```

String processing:

```
std::stringfind()substr()
```

---

# 5. Timeouts

Big scanner issue:

```
connect()
```

can hang.

Learn:

```
setsockopt()select()poll()
```

or

```
non-blocking sockets
```

Mental model:

```
Open Port     -> respondsClosed Port   -> immediate failFiltered Port -> no response
```

Timeouts distinguish these cases.

---

# 6. Thread Pool

A common beginner mistake:

```
for(int p=1;p<65535;p++){    std::thread(scan,p);}
```

This can create **65,000 threads** 😅

Instead:

```
8 worker threads
```

or

```
16 worker threads
```

that pull ports from a queue.

Concepts:

```
queuemutexcondition_variable
```

This is your first taste of concurrent systems programming.

---

# 7. STL You'll Actually Use

For this project:

```cpp
std::vector
std::queue
std::thread
std::mutex
std::unordered_map
std::string

```
