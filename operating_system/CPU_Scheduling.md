# CPU Scheduling

1. https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/6_CPU_Scheduling.html
2. 进程管理 Process_Scheduling: https://blog.csdn.net/weixin_37289816/article/details/55518203

## 6.1 Basic Concepts

1. Almost all have waste cpu time waiting for I/O, so a scheduling system should allow one process to use the CPU while another is waiting for I/O, thereby making full use of otherwise lost CPU cycles.
2. The challenge is to make the overall system as "efficient" and "fair" as possible

### 6.1.1 CPU-I/O Burst Cycle

**Think of a “burst” as a brief stretch of “run as fast as you can go until you can’t.”**

1. **CPU Burst**:
   It is the amount of time, a process uses the CPU until it starts waiting for some input or interrupted by some other process.
2. **I/O Burst**:
   It is the amount of time, a process waits for input-output before needing CPU time.

### 6.1.2 CPU Scheduler(Short-term Scheduler)

1. when CPU is idle, it is CPU Scheduler's job to select process from ready queue to run next.

### 6.1.3. Preemptive Scheduling 抢占式调度

CPU scheduling decisions take place under one of four conditions:

1. When a process switches from the **running state** -> **waiting state**, such as for an I/O request or invocation of the wait( ) system call.
2. When a process switches from the **running state** -> **ready state**, for example in response to an interrupt.
3. When a process switches from the **waiting state** -> **ready state**, say at completion of I/O or a return from wait( ).
4. When a process terminates.

For conditions 1 and 4 there is no choice - A new process must be selected.
For conditions 2 and 3 there is a choice - To either continue running the current process, or select a different one.

#### 1. what is Preemptive Scheduling?

If scheduling takes place only under conditions 1 and 4, the system is said to be **non-preemptive**, or **cooperative**. 让进程运行直到 terminate 或 block 的调度方式. Otherwise it is preemptive.

**Note:**
that preemptive scheduling is only possible on hardware that supports a timer interrupt.

#### 2. problems Preemptive Scheduling may cause

1. preemptive scheduling can cause problems when two processes share data, because one process may get interrupted in the middle of updating shared data structures.

2. preemptive scheduling can also be a problem if the kernel is busy implementing a system call ( e.g. updating critical kernel data structures ) when the preemption occurs. Most modern UNIXes deal with this problem by making the process **wait** until the system call has either completed or blocked before allowing the preemption. Unfortunately this solution is problematic for real-time systems, as real-time response can no longer be guaranteed.

### 6.1.4 Dispatcher

## 6.2 Scheduling Criteria 调度标准

There are several different criteria to consider when trying to select the "best" scheduling algorithm for a particular situation and environment, including:

1. CPU utilization (cpu 利用率)
   Ideally the CPU would be busy 100% of the time, so as to waste 0 CPU cycles. On a real system CPU usage should range from 40% ( lightly loaded ) to 90% ( heavily loaded. )

2. Throughout (吞吐量)
   number of process complete per unit time

3. Turnaround time (周转时间)
   Time required for a particular process to complete, from submission time to completion.
   是指从作业提交给系统开始，到作业完成为止的这段时间间隔，常用于评价批处理系统的性能。

4. wait time
   amount of time that a process spends waiting in the ready queue.

5. response time
   Elapsed time between the submission of a request until there is output / the time from the submision of a request until the first response is produced.

## 6.3 Scheduling Algorithms

### 6.3.1 First-Come First-Serve Scheduling, FCFS

#### 1. What is FCFS?

FCFS is very simple - Just a FIFO queue, like customers waiting in line at the bank

#### 2. What is the problem of FCFS?

1. FCFS can yield some very **long average wait times**, particularly if the first process to get there takes a long time.

2. **convoy effect(护送效应)**:
   one CPU intensive process blocks the CPU, a number of I/O intensive processes can get backed up behind it(in ready queue), leaving the I/O devices idle. The whole operating system slow down.
   可能有个 cpu bound process 占着 cpu，其他很多的 i/o bounded process 已经完成了 i/o 在等待被 cpu 执行，导致 i/o 也被闲置，整个操作系统都 slow down

### 6.3.2 Shortest-Job-First Scheduling, SJF

#### 1. What is SJF?

1. The idea behind the SJF algorithm is to pick the quickest fastest little job that needs to be done, get it out of the way first, and then pick the next smallest fastest job to do next.
   ( Technically this algorithm picks a process based on the next shortest CPU burst, not the overall process time. )
2. SJF can be either preemptive or non-preemptive. Preemption occurs when a new process arrives in the ready queue that has a predicted burst time shorter than the time remaining in the process whose burst is currently on the CPU. Preemptive SJF is sometimes referred to as shortest remaining time first scheduling.

#### 2. What is the problem of SJF?

1. SJF can be proven to be the fastest scheduling algorithm, but it suffers from the problem that you can't really know how long the next CPU burst will be.
   - Predict: The next CPU burst is generally predicted as an exponential average of the measured lengths of previous CPU bursts.

### 6.3.3 Priority Scheduling

#### 1. What is Priority Scheduling?

1. Priority scheduling is a more general case of SJF, in which each job is assigned a priority and the job with the highest priority gets scheduled first.
2. Note that in practice, priorities are implemented using integers within a fixed range, but there is no agreed-upon convention as to whether "high" priorities use large numbers or small numbers.
3. Priorities can be assigned either internally or externally.
   - Internal priorities are assigned by the OS using criteria such as average burst time, ratio of CPU to I/O activity, system resource use, and other factors available to the kernel.
   - External priorities are assigned by users, based on the importance of the job, fees paid, politics, etc.
4. Priority scheduling can be either preemptive or non-preemptive.

#### 2. What is the problem of Priority Scheduling?

1. **indefinite blocking/starvation:**
   a low-priority task can wait forever because there are always some other jobs around that have higher priority.
2. **Aging:**
   One common solution to this problem is aging, in which priorities of jobs increase the longer they wait. Under this scheme a low-priority job will eventually get its priority raised high enough that it gets run.

### 6.3.4 Round Robin Scheduling

#### 1. what is Round Robin Scheduling?

Round robin scheduling is similar to FCFS scheduling, except that CPU bursts are assigned with limits called time quantum. In practice, most modern systems have time quanta ranging from 10 to 100 milliseconds.

1. Every process take turns to run a limit time quantum. When a process is given the CPU, a timer is set for whatever value has been set for a time quantum.

- If the process finishes its burst before the time quantum timer expires, then it is swapped out of the CPU just like the normal FCFS algorithm.
- If the timer goes off first, then the process is swapped out of the CPU and moved to the back end of the ready queue.

#### 2. what is the problem of RR

1. The performance of RR is sensitive to the time quantum selected. If the quantum is large enough, then RR reduces to the FCFS algorithm; If it is very small, then each process gets 1/n of the cpu time(in chunks) and share the CPU equally.

2. a real system invokes overhead for every context switch, and the smaller the time quantum the more context switches there are.

   > In computers, overhead refers to the processing time required by system software

3. Turn around time also varies with quantum time. Turnaround time is minimized if most processes finish their next cpu burst within one time quantum.

### 6.3.5 Multilevel Queue Scheduling

1. Categorized process to separate queues, implement whatever scheduling algorithm that is most appropriate to that type of job.
2. Scheduling must also be done between queues. (priority/RR)
3. jobs cannot switch from queue to queue. Once assigned, they belong to the queue until finished.

### 6.3.6 Multilevel Feedback-Queue Scheduling

1. Mutilevel feedback queue scheduling is the same as mutilevel queue schedule except job may move from one queue to another

## 6.4 Thread Scheduling
