1. Pipe 匿名管道(unnamed pipe)
  * 是 half-duplex 的, 即数据是单向的流动的(full-duplex 是双向的). 如果需要 full-duplex 需要建立两个管道*实质就是 kernal 的一个缓冲区
  * 没有名字(路径)所以只能用于有亲缘关系的进程之间(parent/child process)
  * 在开始传输前需要规定多少个字节是一个消息
  * 无名管道阻塞
