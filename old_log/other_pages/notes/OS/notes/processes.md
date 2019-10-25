### Why Multiprocess
- reduce latency
### Process
- own
    - address
    - open file
    - virtual CPU
### original Unix paper CHeck
- PASS some contents
## Kernel View
### implementing processes
- PCB
- state of the process
- waiting: needs async event - e.g) disk operation
- ready: can run, but kernel has chosen different process to run
- Kernel
    - idle cpu run
    - single process run
    - if other: scehduling
### Scheduling
- how to pick
    - FIFO
    - Priority: some thread -> CPU
    - no universial policy
- preemption
- context switch
    - P_0
    - PCB_0 save state
    - reload state from PCB_1
    - save state into PCB_1
    - reloade state from PCB_0
    - ## Machine dependent
    - save/restore: **expensive**
    - cache misses.
## Threads
- DEF: schedulable execution context
    - program counter, stack, registers
    - under code+register+stacks +++ thread
    - multi - threaded programs
        - run in same process's address place
### Why threads?
#### Concurrency
- ligth weight
- all threads in one process: share memory, file descriptors
#### allow Process to use multiple CPUs or cores
#### allow program to overlap I/O and computation
- e.g) emacs + gcc, threaded web server
#### most kernels have threads too

### Thread package API
- [src](http://www.scs.stanford.edu/17wi-cs140/sched/readings/birrell.pdf)
- preemptive / non pre.
    - pre: more **race** conditions
    - non: can't take advantage of multiple CPUs
### Kernel Threads
- tail(kernel thread) on that wavy thread(user thread)
- faster than process, still **heavy**
#### limitations
- 10x-30x slower when implemented in Kernel
- every thread operation must go through kernel
- heavy-weight memory requirement   
### Alternative: User threads
- implement as user-level library
- *thread_create*,,,: just library functions
#### implementing user-level threads:
- allocate a new stack for each *thread_create*
- keep a queue
- replace networking syscalls(r/w..)
- schedule: setitimer
- e.g) web server
    - thread calls *read* to get data
    - "fake" *read* fucntion makes *read* syscall in non-blocking mode
    - on timer or idle: check which connections have new data
### Thread Implementatio details
- register divided into 2 groups
- stack structure
    - sp register: base of stack
    - local var
    - callee-saved registers
        - function arguments go in caller-saved regs
    - fp
    - old frame ptr
    - return addr
    - Call arguments
#### Bg: procedure calls
- procdure call
    - save active caller registers
    - call foo(pushes pc)
        - save used called rg
        - ..do stuff..
        - restore callee saved rg
        - jump back to calling func
    - resture caller rg
#### Pintos
- implments user process on top of its own threads
- per-trhead state control block sturcture
- swtich function
- thread_create: create new stack
#### i386
- check the image
- %esp pointer moves from one stack to the other
- and callee-saved registers restored
#### user threads on kernel threads
- multiple kernel level threads per process
- n:m therading
    - n user threads per m kernel threads
    - limitations: same problem w/ n:1 threads
        - blocked threads, deadlock
        - hard to keep same # kthrheads as available CPUs
            - Kernel knows # of CPUs, which are blocked
            - but tries to hide from applications
            - Kernel doesn't know the relative importance of threads
### LESSONS
- best thread: as a library
- better kernel interfaces suggested
- concurrency: increases complexity