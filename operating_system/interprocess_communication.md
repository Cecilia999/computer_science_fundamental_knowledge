# InterProcess Communication

### 1. Pipe 匿名管道(unnamed pipe)

- 是 half-duplex 的, 即数据是单向的流动的(full-duplex 是双向的). 如果需要 full-duplex 需要建立两个管道\*实质就是 kernal 的一个缓冲区
- 没有名字(路径)所以只能用于有亲缘关系的进程之间(parent/child process 或者有共同的 parent)
- 在开始传输前需要规定多少个字节是一个消息
- 无名管道阻塞
  - 无需显示打开，创建时直接返回 fd 文件描述符
  - 读写是需要确认对方存在，否则退出
  - 写入无名管道的数据超过其最大值，write 操作将阻塞
  - 管道中没有数据，read 操作将阻塞

### 2. FIFO 有名管道

- 提供了路径名，以磁盘文件的形式存在，可用于本机任意两个进程之间
- Pipe 都是先进先出(FIFO), 不支持 seek()等文件定位, 不支持定位读写
- 有名管道阻塞
  - 有名管道在打开时需要确实对方的存在，否则将阻塞 -> 以读方式打开某管道，在此之前必须一个进程以写方式打开管道，否则阻塞

- [**匿名管道 vs 有名管道 c 语言实践**](http://blog.chinaunix.net/uid-26833883-id-3227144.html)

### [3. 信号(Signal)](https://blog.csdn.net/weixin_38663899/article/details/86136682)

- 信号是 Linux 系统中用于进程间互相通信或者操作的一种机制，信号可以在任何时候发给某一进程，而无需知道该进程的状态。
- 如果该进程当前并未处于执行状态，则该信号就有内核保存起来，知道该进程回复执行并传递给它为止。
- 如果一个信号被进程设置为阻塞，则该信号的传递被延迟，直到其阻塞被取消是才被传递给进程。
- [**Linux 系统中常用信号：**](<https://en.wikipedia.org/wiki/Signal_(IPC)>)

  > SIGHUP：用户从终端注销，所有已启动进程都将收到该进程。系统缺省状态下对该信号的处理是终止进程.  
  > SIGINT：程序终止信号。程序运行过程中，按 Ctrl+C 键将产生该信号。  
  > SIGQUIT：程序退出信号。程序运行过程中，按 Ctrl+\\键将产生该信号。   
  > SIGBUS 和 SIGSEGV：进程访问非法地址。   
  > SIGFPE：运算中出现致命错误，如除零操作、数据溢出等。  
  > SIGKILL：用户终止进程执行信号。shell 下执行 kill -9 发送该信号。  
  > SIGTERM：结束进程信号。shell 下执行 kill 进程 pid 发送该信号。  
  > SIGALRM：定时器信号。  
  > SIGCLD：子进程退出信号。如果其父进程没有忽略该信号也没有处理该信号，则子进程退出后将形成僵尸进程。  

- 信号来源
  - 硬件来源：用户按键输入 Ctrl+C 退出、硬件异常如无效的存储访问等
  - 软件终止：终止进程信号、其他进程调用 kill 函数、软件异常产生信号

##### 参考：
https://blog.csdn.net/weixin_38663899/article/details/86136682  
https://en.wikipedia.org/wiki/Signal_(IPC)  

### 4. Message queue 消息队列

- Message queue is a linked list of messages stored within the kernal and identify by a message queue identifier 消息队列是存放在内核中的消息链表，每个消息队列由消息队列标识符表示
- A new queue is created or an existing queue opened by `<msgget()>`
- New messages are added to the end of a queue by `<msgsnd()>`
- Messages are fetched from a queue by `<msgrcv()>`
- message queue vs pipe：都遵循先进先出 FIFO
  - 无名管道：只存在于内存中的文件
  - 有名管道：存在于实际的磁盘介质或者文件系统
  - 消息队列：
    - 存在于内核，只有内核重启/人工删除才会被删除
    - 允许一个 or 多个进程向 message queue 写入/读取 message
    - 可以实现消息的随机查询, message 不一定要以 FIFO,也可以按消息的类型读取.比 FIFO 更有优势
- 目前主要有两种类型的消息队列：POSIX 以及 System V, System V 目前被大量使用

##### 参考：

[https://www.geeksforgeeks.org/ipc-using-message-queues/](https://www.geeksforgeeks.org/ipc-using-message-queues/)  
[https://blog.csdn.net/yang_yulei/article/details/19772649](https://blog.csdn.net/yang_yulei/article/details/19772649)

### 5. shared memory 共享内存

- two or more process can access the common memory 多个进程可以可以直接读写同一块内存空间，是最快的可用IPC形式
- pipe & fifo & message queue vs shared memory:
  - **pipe/fifo/message queue**
    - Server reads from the input file.
    - The server writes this data in a message using pipe/fifo/message queue.
    - The client reads the data from the IPC channel, again requiring the data to be copied from kernel’s IPC buffer to the client’s buffer.
    - Finally the data is copied from the client’s buffer.
    - 一共需要copy4次，2read + 2write

  - **shared memory**
    - 共享内存则只拷贝两次数据：一次从输入文件到共享内存区，另一次从共享内存区到输出文件
    - 内核专门留出了一块内存区，可以由需要访问的进程将其映射到自己的私有地址空间。进程就可以直接读写这一块内存而不需要进行数据的拷贝，从而提高效率
    - 进程之间在共享内存时，并不总是读写少量数据后就解除映射，有新的通信时，再重新建立共享内存区域。而是保持共享区域，直到通信完毕为止


##### 参考：

https://www.geeksforgeeks.org/ipc-shared-memory/  
https://www.cnblogs.com/linuxbug/p/4882776.html

### 6. Semaphore 信号量



#### 参考文献：

https://www.jianshu.com/p/c1015f5ffa74
https://opensource.com/article/19/4/interprocess-communication-linux-channels
https://www.guru99.com/inter-process-communication-ipc.html
https://www.geeksforgeeks.org/ipc-using-message-queues/
