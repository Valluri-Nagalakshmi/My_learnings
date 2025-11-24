# Linux Signals — Full Explanation with Code Snippets

##  1. What is a Signal?
A **signal** is a software interrupt sent to a process or thread to notify it that an event has occurred.

A signal:
- Interrupts normal program flow  
- Forces the process to react immediately  

###  Small Code Snippet: Print PID
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    printf("My PID: %d\n", getpid());
    while(1);
}
```

---

##  2. Why Do We Use Signals?

Signals are used for:
- Interrupting a process  
- Terminating a process  
- Pausing/resuming  
- Error notification  
- Child exit notification  
- Timer expiration  
- IPC  

### Example: Ctrl+C → SIGINT
```c
// Press Ctrl+C during execution to generate SIGINT
```

---

##  3. How Are Signals Generated?

###  1. Kernel Exceptions — SIGSEGV
```c
int *p = NULL;
*p = 10;   // SIGSEGV
```

###  2. User Keyboard Inputs
- Ctrl+C → SIGINT  
- Ctrl+Z → SIGTSTP  

###  3. System Calls
```c
kill(1234, SIGTERM);   // Send signal to another process
raise(SIGINT);         // Send signal to yourself
```

###  4. Timers → SIGALRM
```c
#include <unistd.h>
int main() {
    alarm(2);
    while(1);
}
```

###  5. Hardware-triggered Signals
```c
int x = 1/0;   // SIGFPE
```

---

##  4. When Do Signals Occur?

Signals occur when:
- Invalid memory access  
- User presses Ctrl+C  
- kill() or raise() is invoked  
- Timer expires  
- Child exits  
- Terminal disconnects  

---

##  5. Types of Signals

### 🔹 A. Standard Signals
Examples: SIGINT, SIGTERM, SIGKILL, SIGSEGV, SIGSTOP, SIGCHLD

List all:
```bash
kill -l
```

### 🔹 B. Real-Time Signals
Range:
```bash
SIGRTMIN → SIGRTMAX
```

Send RT signal with data:
```c
sigqueue(pid, SIGRTMIN, (union sigval){ .sival_int = 100 });
```

---

##  6. Signal Life Cycle

### Stage 1 — Generation  
via kill(), raise(), exceptions, timers, user input

### Stage 2 — Delivery  
Delivered only when process is runnable and unblocked

### Stage 3 — Handling  
Handled by default, ignore, or custom handler

---

##  7. How Signals Are Handled

###  Default Handling
- SIGSEGV → crash  
- SIGINT → terminate  

###  Ignore a Signal
```c
#include <signal.h>
#include <unistd.h>

int main() {
    signal(SIGINT, SIG_IGN);
    while(1) sleep(1);
}
```

###  Custom Handler
```c
#include <stdio.h>
#include <signal.h>

void handler(int sig) {
    printf("Caught signal %d\n", sig);
}

int main() {
    signal(SIGINT, handler);
    while(1);
}
```

---

##  8. What is `signal()`?

Simple but old API.

```c
signal(SIGINT, handler);
```

Limitations:
- Not portable  
- No sender info  
- May behave inconsistently  

---

##  9. What is `sigaction()`?

Provides full, modern control.

```c
#include <stdio.h>
#include <signal.h>

void handler(int sig, siginfo_t *info, void *ctx) {
    printf("Caught %d from PID %d\n", sig, info->si_pid);
}

int main() {
    struct sigaction sa;
    sa.sa_flags = SA_SIGINFO;
    sa.sa_sigaction = handler;
    sigemptyset(&sa.sa_mask);

    sigaction(SIGINT, &sa, NULL);
    while(1);
}
```

---

##  10. What is `sigprocmask()`?

Used to block/unblock signals.

### Block SIGINT
```c
sigset_t set;
sigemptyset(&set);
sigaddset(&set, SIGINT);
sigprocmask(SIG_BLOCK, &set, NULL);
```

### Unblock SIGINT
```c
sigprocmask(SIG_UNBLOCK, &set, NULL);
```

---

##  Quick Summary with Code

| Concept | Purpose | Code Example |
|--------|---------|--------------|
| signal() | Simple handler | `signal(SIGINT, handler);` |
| sigaction() | Advanced handling | Uses `SA_SIGINFO` |
| sigprocmask() | Block/unblock | `sigprocmask(SIG_BLOCK, ...)` |
| raise() | Send to self | `raise(SIGINT);` |
| kill() | Send to another process | `kill(pid, SIGTERM);` |
| sigqueue() | RT signal w/data | `sigqueue(pid, SIGRTMIN, val);` |
| alarm() | Timer | `alarm(5);` |

---
# Linux Signals — Frequently Asked Questions & Answers

##  1. What is a signal?
A signal is an asynchronous software interrupt sent to a process or thread when an event occurs.

---

##  2. Why are signals used?
- Interrupt a process  
- Terminate a process  
- Pause/resume  
- Error notification  
- Child process notification  
- Timer events  
- IPC  

---

##  3. How are signals generated?
- Kernel exceptions (SIGSEGV, SIGFPE)  
- User keyboard (Ctrl+C, Ctrl+Z)  
- kill(), raise(), sigqueue()  
- Timers (alarm())  
- Hardware faults translated by kernel  

---

##  4. Interrupt vs Exception vs Signal
| Type | Source | Target | Example |
|------|--------|--------|----------|
| Interrupt | Hardware | Kernel | Keyboard press |
| Exception | CPU | Kernel | Divide-by-zero |
| Signal | Kernel/User | Process | SIGINT |

---

##  5. What is default action of a signal?
- Terminate  
- Terminate + core dump  
- Ignore  
- Stop  
- Continue  

---

##  6. Why can’t SIGKILL & SIGSTOP be caught?
They must always work to kill or stop misbehaving processes. Kernel enforces it.

---

##  7. Standard vs Real-time signals
### Standard:
- Not queued  
- No data  
- Basic signals (SIGINT, SIGTERM)

### Real-time:
- Queued  
- Carry data  
- Range: SIGRTMIN → SIGRTMAX  

---

##  8. What is pending signal?
A blocked signal that has been generated but not yet delivered.

---

##  9. What is signal disposition?
How a signal is handled:
- Default  
- Ignore  
- Custom handler  

---

##  10. How does a signal handler work?
A function is invoked when a signal is delivered.

---

##  11. What is `signal()`?
Simple but unreliable handler installation function.

---

##  12. Why use `sigaction()`?
- Portable  
- Supports SA_SIGINFO  
- Reliable  
- Can block other signals  
- Allows syscall restart  

---

##  13. What is SA_RESTART?
Automatically restarts interrupted system calls.

---

##  14. What is async-signal-safe?
Functions safe to call inside handlers (write, _exit, sigaction).

Unsafe (printf, malloc) may deadlock.

---

##  15. Why is printf() unsafe inside handler?
It uses internal locks → may deadlock if interrupted.

---

##  16. What is `sigprocmask()`?
Used to block/unblock signals.

---

##  17. What happens when a blocked signal arrives?
It becomes **pending**.

---

##  18. Signals in multithreading
- Delivered to one thread  
- Handlers shared  
- Masks are per-thread  

---

##  19. What is dedicated signal-handling thread?
A thread that uses sigwait(), sigwaitinfo(), etc. to safely process signals.

---

##  20. Signal life cycle
1. Generation  
2. Delivery  
3. Handling  

---

##  21. When use kill()?
To send signal to process or group.

---

##  22. Difference kill() vs raise()
- kill() → another process  
- raise() → self  

---

##  23. What is sigqueue()?
Send real-time signal with data.

---

##  24. What is SIGCHLD?
Indicates child status change.

---

##  25. What is SIGPIPE?
Writing to closed pipe → default terminate.

---

##  26. SIGTERM vs SIGKILL
| Signal | Meaning | Catchable |
|--------|---------|-----------|
| SIGTERM | Request quit | Yes |
| SIGKILL | Force quit | No |

---

##  27. What is segmentation fault?
SIGSEGV on invalid memory access.

---

##  28. What is SIGHUP?
Terminal disconnect; daemons use to reload config.

---

##  29. What is core dump?
Memory snapshot created when process crashes.

---

##  30. Uses of real-time signals
Queued, ordered, carry data — for real-time tasks.

---

##  31. What is siginfo_t?
Structure with sender PID, value, reason, fault address.

---

##  32. What are unreliable signals?
Old-style signal() with inconsistent behavior.

---

##  33. What is reentrant function?
Safe to call multiple times even during interruption.

---

##  34. How kernel delivers signals?
Checks mask, state, and injects handler on next user-mode entry.

---

##  35. Can signal interrupt syscalls?
Yes → syscalls return EINTR unless SA_RESTART is used.

---

##  36. What is zombie process?
Terminated, but parent hasn't collected its exit status.

---

##  37. Do children inherit pending signals?
No.  
But they inherit signal mask and handlers.

---

##  38. Can a signal interrupt another handler?
Yes, if unblocked and SA_NODEFER is set.

---

##  39. Are signals merged?
Standard → merged  
Real-time → queued individually  

---
