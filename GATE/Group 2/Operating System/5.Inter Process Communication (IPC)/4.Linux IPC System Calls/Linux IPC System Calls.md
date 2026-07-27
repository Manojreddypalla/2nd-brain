# Linux IPC System Calls with C Examples

---

# 1. Pipes

## pipe()

### Prototype

```c
#include <unistd.h>

int pipe(int fd[2]);
```

### Purpose

Creates an anonymous pipe.

- `fd[0]` → Read End
- `fd[1]` → Write End

### Example

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    int fd[2];
    pipe(fd);

    write(fd[1], "Hello", 5);

    char buffer[10];
    read(fd[0], buffer, 5);
    buffer[5] = '\0';

    printf("%s\n", buffer);

    return 0;
}
```

### Workflow

```
pipe()
      ↓
write()
      ↓
Kernel Pipe
      ↓
read()
```

---

# 2. Named Pipe (FIFO)

## mkfifo()

### Prototype

```c
#include <sys/stat.h>

int mkfifo(const char *pathname, mode_t mode);
```

### Example

```c
#include <sys/stat.h>

int main() {
    mkfifo("myfifo", 0666);
}
```

Creates:

```
myfifo
```

which behaves like a pipe.

---

# 3. Shared Memory

## shmget()

### Prototype

```c
#include <sys/ipc.h>
#include <sys/shm.h>

int shmget(key_t key,
           size_t size,
           int shmflg);
```

### Example

```c
int shmid;

shmid = shmget(1234, 1024, IPC_CREAT | 0666);
```

Creates a 1024-byte shared memory segment.

---

## shmat()

### Prototype

```c
void *shmat(int shmid,
            const void *addr,
            int flag);
```

### Example

```c
char *ptr;

ptr = (char *)shmat(shmid, NULL, 0);

strcpy(ptr, "Hello");
```

Attaches the shared memory to the process.

---

## shmdt()

### Prototype

```c
int shmdt(const void *addr);
```

### Example

```c
shmdt(ptr);
```

Detaches the memory.

---

## shmctl()

### Prototype

```c
int shmctl(int shmid,
           int cmd,
           struct shmid_ds *buf);
```

### Example

```c
shmctl(shmid, IPC_RMID, NULL);
```

Deletes the shared memory segment.

---

### Workflow

```
shmget()

↓

shmat()

↓

Read / Write

↓

shmdt()

↓

shmctl()
```

---

# 4. Message Queue

## msgget()

### Prototype

```c
#include <sys/msg.h>

int msgget(key_t key,
           int msgflg);
```

### Example

```c
int msgid;

msgid = msgget(1234, IPC_CREAT | 0666);
```

Creates a queue.

---

## msgsnd()

### Prototype

```c
int msgsnd(int msqid,
           const void *msgp,
           size_t msgsz,
           int flag);
```

### Example

```c
struct msg {
    long type;
    char text[100];
};

struct msg m = {1, "Hello"};

msgsnd(msgid, &m, sizeof(m.text), 0);
```

Sends a message.

---

## msgrcv()

### Prototype

```c
ssize_t msgrcv(int msqid,
               void *msgp,
               size_t msgsz,
               long type,
               int flag);
```

### Example

```c
struct msg m;

msgrcv(msgid,
       &m,
       sizeof(m.text),
       1,
       0);
```

Receives a message.

---

## msgctl()

### Prototype

```c
int msgctl(int msqid,
           int cmd,
           struct msqid_ds *buf);
```

### Example

```c
msgctl(msgid,
       IPC_RMID,
       NULL);
```

Deletes the queue.

---

### Workflow

```
msgget()

↓

msgsnd()

↓

Queue

↓

msgrcv()

↓

msgctl()
```

---

# 5. Sockets

## socket()

### Prototype

```c
#include <sys/socket.h>

int socket(int domain,
           int type,
           int protocol);
```

### Example

```c
int sockfd;

sockfd = socket(AF_INET,
                SOCK_STREAM,
                0);
```

Creates a TCP socket.

---

## bind()

### Prototype

```c
int bind(int sockfd,
         const struct sockaddr *addr,
         socklen_t len);
```

### Example

```c
bind(sockfd,
     (struct sockaddr *)&server,
     sizeof(server));
```

Associates socket with IP and Port.

---

## listen()

### Prototype

```c
int listen(int sockfd,
           int backlog);
```

### Example

```c
listen(sockfd, 5);
```

Waits for clients.

---

## accept()

### Prototype

```c
int accept(int sockfd,
           struct sockaddr *addr,
           socklen_t *len);
```

### Example

```c
client = accept(sockfd,
                NULL,
                NULL);
```

Accepts a connection.

---

## connect()

### Prototype

```c
int connect(int sockfd,
            const struct sockaddr *addr,
            socklen_t len);
```

### Example

```c
connect(sockfd,
        (struct sockaddr *)&server,
        sizeof(server));
```

Connects client to server.

---

## send()

### Prototype

```c
ssize_t send(int sockfd,
             const void *buf,
             size_t len,
             int flag);
```

### Example

```c
send(sockfd,
     "Hello",
     5,
     0);
```

---

## recv()

### Prototype

```c
ssize_t recv(int sockfd,
             void *buf,
             size_t len,
             int flag);
```

### Example

```c
char buffer[100];

recv(sockfd,
     buffer,
     sizeof(buffer),
     0);
```

---

## close()

### Prototype

```c
#include <unistd.h>

int close(int fd);
```

### Example

```c
close(sockfd);
```

---

### Server Workflow

```
socket()

↓

bind()

↓

listen()

↓

accept()

↓

send()/recv()

↓

close()
```

---

### Client Workflow

```
socket()

↓

connect()

↓

send()/recv()

↓

close()
```

---

# 6. Signals

## kill()

### Prototype

```c
#include <signal.h>

int kill(pid_t pid,
         int sig);
```

### Example

```c
kill(1234,
     SIGTERM);
```

Sends SIGTERM to process 1234.

---

## signal()

### Prototype

```c
void (*signal(int sig,
              void (*handler)(int)))(int);
```

### Example

```c
signal(SIGINT, handler);
```

Registers a signal handler.

---

## sigaction()

### Prototype

```c
int sigaction(int sig,
              const struct sigaction *act,
              struct sigaction *oldact);
```

### Example

```c
struct sigaction sa;

sa.sa_handler = handler;

sigaction(SIGINT,
          &sa,
          NULL);
```

Modern method for handling signals.

---

## raise()

### Prototype

```c
#include <signal.h>

int raise(int sig);
```

### Example

```c
raise(SIGINT);
```

Sends a signal to the current process.

---

### Workflow

```
kill()

↓

Kernel

↓

Signal

↓

Handler
```

---

# GATE Quick Memory ⭐

| IPC             | System Calls                                      |
| --------------- | ------------------------------------------------- |
| Pipe            | `pipe()`                                          |
| Named Pipe      | `mkfifo()`                                        |
| Shared Memory   | `shmget()` → `shmat()` → `shmdt()` → `shmctl()`   |
| Message Queue   | `msgget()` → `msgsnd()` → `msgrcv()` → `msgctl()` |
| Socket (Server) | `socket()` → `bind()` → `listen()` → `accept()`   |
| Socket (Client) | `socket()` → `connect()`                          |
| Data Transfer   | `send()` ↔ `recv()`                               |
| Signals         | `kill()` · `signal()` · `sigaction()` · `raise()` |