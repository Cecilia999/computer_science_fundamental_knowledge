# Java thread pool - Java 并发编程：线程池的使用

从线程的运行原理我们知道，我们需要一个线程的时候就创建一个新的线程，任务结束后摧毁线程。  
但当并发的线程很多的时候，这种频繁创建和销毁线程的方式会较低系统的效率  
**java 解决的方法是利用线程池，使得线程可以复用，执行完一个任务，并不被销毁，而是继续执行其他的任务 **

### Java 中的 ThreadPoolExecutor 类

    java.uitl.concurrent.ThreadPoolExecutor 类是线程池中最核心的一个类，ThreadPoolExecutor 类的具体实现源码如下：

    ```
    public class ThreadPoolExecutor extends AbstractExecutorService {
        .....
        public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
                BlockingQueue<Runnable> workQueue);

        public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
                BlockingQueue<Runnable> workQueue,ThreadFactory threadFactory);

        public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
                BlockingQueue<Runnable> workQueue,RejectedExecutionHandler handler);

        public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
            BlockingQueue<Runnable> workQueue,ThreadFactory threadFactory,RejectedExecutionHandler handler);
        ...
    }

    ```

### 参考

    https://www.cnblogs.com/dolphin0520/p/3932921.html
