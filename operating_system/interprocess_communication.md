# InterProcess Communication
### 1. Pipe 匿名管道(unnamed pipe)
   * 是 half-duplex 的, 即数据是单向的流动的(full-duplex 是双向的). 如果需要 full-duplex 需要建立两个管道*实质就是 kernal 的一个缓冲区
   * 没有名字(路径)所以只能用于有亲缘关系的进程之间(parent/child process)
   * 在开始传输前需要规定多少个字节是一个消息
   * 无名管道阻塞
     * 无需显示打开，创建时直接返回fd文件描述符
     * 读写是需要确认对方存在，否则退出
     * 写入无名管道的数据超过其最大值，write操作将阻塞
     * 管道中没有数据，read操作将阻塞


### 2. FIFO有名管道
   * 提供了路径名，以磁盘文件的形式存在，可用于本机任意两个进程之间
   * Pipe都是先进先出(FIFO), 不支持seek()等文件定位, 不支持定位读写
   * 有名管道阻塞
     * 有名管道在打开时需要确实对方的存在，否则将阻塞 -> 以读方式打开某管道，在此之前必须一个进程以写方式打开管道，否则阻塞

[**匿名管道vs有名管道c语言实践**](http://blog.chinaunix.net/uid-26833883-id-3227144.html)

### 3.







#### 参考文献：
    https://www.jianshu.com/p/c1015f5ffa74
    https://opensource.com/article/19/4/interprocess-communication-linux-channels


