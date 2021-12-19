# Approaches for Inter-Process Communication

1. https://www.guru99.com/inter-process-communication-ipc.html
2. interprocess communication 不仅指 process 之间的，还包括 thread 之间的
   - process: shared memory + message passing + socket
   - thread: signal + semaphore

## 1. Pipe 匿名管道(unnamed pipe)

Pipe is widely used for communication between two related processes. This is a half-duplex method, so to achieve a full-duplex, we need to create two pipes.

issues:

- Unidirectional or Bidirectional?
- half-duplex or full-duplex?
- must a relationship such as parent-child between processes?
- communicate in network or only on the same machine?

### 1. ordinary pipes(unamed pipe)

1. uni-directional, with a reading end and a writing end.
2. In UNIX ordinary pipes are created with the system call `int pipe( int fd [] )`

   - return 0, success
   - return 1, error
   - fd[] must be allocated before the call, and the values are filled in by the pipe system call:
     - fd[0] is filled in with a file descriptor for the reading end of the pipe
     - fd[1] is filled in with a file descriptor for the writing end of the pipe

3. UNIX pipes are accessible as files, using standard **read(**) and **write()** system calls.

4. Ordinary pipes are only accessible within the process that created them.
   - Typically a parent creates the pipe before forking off a child.
   - **When the child inherits open files from its parent, including the pipe file(s)**, a channel of communication is established.
   - Each process ( parent and child ) should first close the ends of the pipe that they are not using. For example, if the parent is writing to the pipe and the child is reading, then the parent should close the reading end of its pipe after the fork and the child should close the writing end.

### 2. Named pipe (FIFO)

1. Named pipe support bidirectional communication, communiction between non parent-child process, pipes persistence after the process create them exit and multiple process can share a same named pipe, typically one reader and multiple writers.

2. In UNIX, named pipes are termed **FIFO**, and appear as ordinary files in the file system.
   - Created with **mkfifo( )** and manipulated with **read( ), write( ), open( ), close( )**, etc.
   - UNIX named pipes are bidirectional, but half-duplex, so you still need two pipes for bidirectional communications.
   - UNIX named pipes still require that all processes be running on the same machine. Otherwise sockets are used.

## 2. shared memory 共享内存

1. `shmget()`: `int shmget(key_t, size_tsize, int shmflg); `

   - In general the memory to be shared in a shared-memory system is initially within the address space of a particular process, which needs to make system calls in order to make that memory publicly available to other processes.
   - upon successful completion, shmget() returns an identifier for the shared memory segment.

2. `shmat()`: `void *shmat(int shmid ,void *shmaddr ,int shmflg)`;

   - Other processes which wish to use the shared memory must also make their own system calls to attach the shared memory area onto their address space.
   - Before you can use a shared memory segment, you have to attach yourself to it using shmat().
     > shmid is shared memory id.
     > shmaddr specifies specific address to use but we should set it to zero and OS will automatically choose the address.

3. So a few messages must be passed back and forth between the cooperating processes to set up the shared memory access.

4. `shmdt()`: `int shmdt(void *shmaddr);`
   When you’re done with the shared memory segment, your program should **detach** itself from it using shmdt().

5. `shmctl()`: `shmctl(int shmid,IPC_RMID,NULL);`
   when you detach from shared memory,it is not destroyed. So, to destroy shmctl() is used.

- 多个进程可以可以直接读写同一块内存空间，是最快的可用 IPC 形式

## 3. Message queue 消息队列

1. Message queue is a linked list of messages stored within the kernal and identify by a message queue identifier 消息队列是存放在内核中的消息链表，每个消息队列由消息队列标识符表示
2. A new queue is created or an existing queue opened by `<msgget()>`
3. New messages are added to the end of a queue by `<msgsnd()>`
4. Messages are fetched from a queue by `<msgrcv()>`
5. `msgctl()`: It performs various operations on a queue. Generally it is use to destroy message queue.
6. `ftok()`: is use to generate a unique key.

7. message queue vs pipe: follows FIFO

- ordinary pipe：只存在于内存中的文件
- fifo：存在于实际的磁盘介质或者文件系统
- message queue：
  - stored within kernal，只有内核重启/人工删除才会被删除
  - 允许一个 or 多个进程向 message queue 写入/读取 message
  - 可以实现消息的随机查询, message 不一定要以 FIFO,也可以按消息的类型读取.比 FIFO 更有优势

6. 目前主要有两种类型的消息队列：POSIX 以及 System V, System V 目前被大量使用

## 4. Socket

1. The socket mechanism provides a inter-process communication (IPC) way by establishing named contact points between which the communication take place.

2. sockets is created using ‘socket’ system call.

   - Create(): To create a socket
   - Bind(): It’s a socket identification like a telephone number to contact
   - Connect(): Ready to act as a sender
   - Accept(): Confirmation, it is like accepting to receive a call from a sender
   - Write(): To send data
   - Read(): To receive data
   - Close(): To close a connection

3. The socket provides bidirectional FIFO Communication facility over the network. A socket connecting to the network is created at each end of the communication.

4. Each socket has a specific address. This address is composed of an IP address and a port number.

5. Types of Sockets :

   - **Datagram Socket**
     This is a type of network which has connection less point for sending and receiving packets. It is similar to mailbox. The letters (data) posted into the box are collected and delivered (transmitted) to a letterbox (receiving socket).

   - **Stream Socket**
     In Computer operating system, a stream socket is type of interprocess communications socket or network socket which provides a connection-oriented, sequenced, and unique flow of data without record boundaries with well defined mechanisms for creating and destroying connections and for detecting errors. It is similar to phone. A connection is established between the phones (two ends) and a conversation (transfer of data) takes place.

6. **socket 的 3 大特性：ip address、port number、protocol type**

- 套接字的域：它指定套接字通信中使用的网络介质，最常见的套接字域有两种：
  - AF_INET，它指的是 Internet 网络，即 IP address
  - AF_UNIX，表示 UNIX 文件系统，它就是文件输入/输出，而它的地址就是文件名
- socket 的 port#：每一个基于 TCP/IP 网络通讯的程序(进程)都被赋予了唯一的端口和端口号，端口是一个信息缓冲区，用于保留 Socket 中的输入/输出信息，端口号是一个**16**位无符号整数，范围是**0-2^16**
- socket protocol type：tcp/udp/原始 socket

## 5. Signal 信号

1. what is singal?

   - A signal is used in UNIX systems to notify a process that a particular event has occurred.
   - A signal may be received either **synchronously** or **asynchronously**, depending on the source of signal and the reason for the event being signaled.
   - All signals has the 3 patterns:
     1. is generated by the occurrence of a particular event.
     2. is delivered to a process.
     3. Once delivered, the signal must be handled.

2. synchronous signal: are **delivered to the same process** that performed the operation that caused the signal

   - illegal memory access
   - divide by 0

3. asynchronous signal: is generated by an event external to the running process

   - terminating a process with specific keystrokes (such as <control> <C>)
   - having a timer expire.

4. how to handle a signal?

   - every signal may be handled by either a **default signal handler** or a **user defined signal handler**

   - handle signal in single-thread process is easy, the problem is how to handle signals for multithread process? which particular thread to send the signal? or should we send the signal to all threads?
     There are four major options:

     1. Deliver the signal to the thread to which the signal applies.
     2. Deliver the signal to every thread in the process.
     3. Deliver the signal to certain threads in the process.
     4. Assign a specific thread to receive all signals in a process.

     The best choice may depend on which specific signal is involved.

5. UNIX allows individual threads to indicate which signals they are accepting and which they are ignoring. However the signal can only be delivered to one thread, which is generally the first thread that is accepting that particular signal.

6. UNIX provides two separate system calls, **kill( pid, signal )** and **pthread_kill( tid, signal )**, for delivering signals to processes or specific threads respectively.

7. Windows does not support signals, but they can be emulated using Asynchronous Procedure Calls ( APCs ). APCs are delivered to specific threads, not processes.

8. 信号是 Linux 系统中用于进程间互相通信或者操作的一种机制，信号可以在任何时候发给某一进程，而无需知道该进程的状态。
9. 如果该进程当前并未处于执行状态，**内核会将该信号保存起来，直到该进程恢复执行并传递给它为止**。
10. 如果一个信号被进程设置为阻塞，则该信号的传递被延迟，直到其阻塞被取消才会传递给进程。
11. [**Linux 系统中常用信号：**](<https://en.wikipedia.org/wiki/Signal_(IPC)>)

> SIGHUP：用户从终端注销，所有已启动进程都将收到该进程。系统缺省状态下对该信号的处理是终止进程.  
> SIGINT：程序终止信号。程序运行过程中，按 **Ctrl+C** 键将产生该信号。  
> SIGQUIT：程序退出信号。程序运行过程中，按**Ctrl+\\**键将产生该信号。  
> SIGBUS 和 SIGSEGV：进程访问非法地址。  
> SIGFPE：运算中出现致命错误，如除零操作、数据溢出等。  
> SIGKILL：用户终止进程执行信号。shell 下执行 **kill -9** 发送该信号。  
> SIGTERM：结束进程信号。shell 下执行 kill 进程 pid 发送该信号。  
> SIGALRM：定时器信号。  
> SIGCLD：子进程退出信号。如果其父进程没有忽略该信号也没有处理该信号，则子进程退出后将形成僵尸进程。

12. 信号来源

- 硬件来源：用户按键输入 Ctrl+C 退出、硬件异常如无效的存储访问等
- 软件终止：终止进程信号、其他进程调用 kill 函数、软件异常产生信号

## 6. Semaphore 信号量

1. what is semaphore
   Semaphore is simply an integer variable that is shared between threads. This variable is used to solve the critical section problem and to achieve process synchronization in the multiprocessing environment.

   - 什么是 critical section problem 临界区问题
     假设现有 n 个进程(P1, P2,…,Pn), 每个进程都如图所示, 拥有一个可以修改共享变量(变量, 文件, 数据库表等)的临界区(critical section), 要求任何一个进程在临界区执行时, 其他都不能执行.

2. A **semaphore S** is an integer variable that, apart from initialization, is accessed only through two standard **atomic** operations: wait() and signal().

- 操作 wait() 最初称为 P（荷兰语 proberen，测试）；
- 操作 signal() 最初称为 V（荷兰语 verhogen，增加）；

```
wait(S){
    while (S <= 0)
        ;// busy wait
    S--；
}
```

```
signal(S) {
    S++;
}
```

3. All the modifications to the value of the semaphore in the wait () and signal() operations is atomic and must be executed without interruption.

   - atomic means that variable on which read, modify and update happens at the same time with no preemption i.e. in-between read, modify and update no other operation is performed that may change the variable.

4. Semaphores are of two types:

   - Binary Semaphore
     This is also known as **mutex lock**. It can have only two values – 0 and 1. Its value is initialized to 1. It is used to solve critical section problems for multiple processes.

   - Count Semaphore
     has value can range over an unrestricted domain. It is used to control access to a resource that has multiple instances. The semaphore is initialized to the number of resources available. Each process that wishes to use a resource performs a wait() operation on the semaphore (thereby decrementing the count). When a process releases a resource, it performs a signal() operation (incrementing the count). When the value for the semaphore goes to 0, all resources are being used. After that, processes that wish to use a resource will block until the count becomes greater than O.

5. Mutual-exclusion implementation with semaphores

```
do{
    waiting(mutex);
    //critical section

    signal(mutex);
    //remainder section
}while(true)
```

**Count Semaphore 与 Binary Semaphore 之间的区别**：

1. 互斥量用于线程的互斥，信号量用于线程的同步。这是互斥量和信号量的根本区别，也就是互斥和同步之间的区别。
2. 互斥量值只能为 0/1，信号量值可以为非负整数。
3. 互斥量的加锁和解锁必须由同一线程分别对应使用，信号量可以由一个线程释放，另一个线程得到。
