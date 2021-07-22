# java virtual machine

## 1. 什么是 jvm

- Java 虚拟机（JVM）是运行 Java 程序的软件环境，用于解析和运行 java 程序。
- 在运行 Java 程序时，Java 源文件被编译成字节码文件(.class file)后，会启动 JVM，然后由它来负责解析和执行 字节码文件，最终形成 JVM 可以直接使用的 Java 类型的过程。
- JVM 把 Java 字节码程序和具体的硬件平台以及操作系统环境分隔开来，只要在不同的计算机上安装了针对特定平台的 JVM，Java 程序就可以运行，而不用考虑当前具体的硬件平台及操作系统环境，也不用考虑字节码文件是在何种平台上生成的。
- Java 解释器 (execute engine)是 Java 虚拟机的一部分。

> note：Java 程序通过 JVM 可以实现跨平台特性，但 JVM 是不跨平台的。也就是说，不同操作系统之上的 JVM 是不同的，Windows 平台之上的 JVM 不能用在 Linux 平台，反之亦然。

## 2. jvm 的结构：

![Alt text](../image/jvm_architecture.jpg)

### 2.1 [类加载器 (class loader) + jvm 类加载的过程/机制](类加载子系统.md)

### 2.2 [运行时数据区 runtime data area](运行时数据区.md)

## 3. JVM 的对象分配规则

- Young Generation（新生代）：  
  分为：Eden 区和 Survivor 区，Survivor 区有分为大小相等的 From Space 和 To Space。
- Old Generation（老年代）：  
  Tenured 区，当 Tenured 区空间不够时， JVM 会在 Tenured 区进行 major collection。
- Minor GC：  
  新生代 GC，指发生在新生代的垃圾收集动作，因为 java 对象大多都具备朝生夕死的特性，所以 Minor GC 非常频繁，一般回收速度也比较快。
- Major GC：  
  发生老年代的 GC，对整个堆进行 GC。出现 Major GC，经常会伴随至少一次 Minor GC（非绝对）。MajorGC 的速度一般比 minor GC 慢 10 倍以上。
- Full GC：  
  整个虚拟机，包括永久区、新生区和老年区的回收。

1. 对象优先分配在 Eden 区，如果 Eden 区没有足够的空间时，虚拟机执行一次 Minor GC**垃圾回收**

2. 大对象和长期存活的对象进入老年代

   - 大对象直接进入老年代（大对象是指需要大量连续内存空间的对象）。这样做的目的是避免在 Eden 区和两个 Survivor 区之间发生大量的内存拷贝（新生代采用复制算法收集内存）。
   - 长期存活的对象进入老年代。（即经历过多次垃圾回收仍存在的对象）虚拟机为每个对象定义了一个年龄计数器，如果对象经过了 1 次 Minor GC（年轻代收集）那么对象会进入 Survivor 区，之后每经过一次 Minor GC 那么对象的年龄加 1，直到达到阀值对象进入老年区。

3. Survivor 中年龄一致的对象所占内存大小，大于幸存者区空间一半时，则大于等于此年龄的对象全部进入老年代 ->> 动态判断对象的年龄

4. 每次进行 Minor GC 时，JVM 会计算 Survivor 区移至老年区的对象的平均大小，如果这个值大于老年区的剩余值大小则进行一次 Full GC ->> 空间分配担保

## 4. jvm 垃圾回收

1. 原理：
   1）C++由构造函数 allocate object in heap(堆) and has a pointer in stack(栈) point to it. 当程序结束的时候跑析构函数(destructor)把 heap memory 释放&stack pt point to null.

   2） Java 没有析构函数而是由 garbage collector 来实现垃圾回收. GC 会 scan heap 然后释放没有 pointer 指向和没有被 reference(引用)的 object.

   3） JVM 控制 GC, 当 JVM 发现 memory is low 的时候会 run garbage collector.

   4） 强引用(strong ref): 永远不会被 GC  
   软引用(soft ref): 内存溢出的时候回收  
   弱引用(weak ref): 第二次垃圾回收的时候回收  
   虚引用(phantom ref): 每次垃圾回收的时候回收, 无法通过引用取到对象值

2. 算法：
   1） 标记-清除算法(mark-sweep): 扫两次，一次标记，一次清除  
   缺点是会产生碎片化的内存导致需要 allocate 一个很大的对象时可能找不到足够大的一块内存只能再次垃圾回收。

   2） 复制算法(copying): 把一块内存分成相同大的两块, 每次内存空间用完了就垃圾回收把还存活的重新整理复制到另外一边。不会导致内存碎片化，实现简单  
   缺点是内存变成了原来的一半，而且如果对象的存活率很高的话要复制的东西很多

   3） 标记-整理算法(mark-compact):
   标记可回收，然后全部整理到内存的一边，清除剩下的位置，实现比复制算法要难一点(pointer 位置的整理

   4） 分区算法：
   把内存分成 n 个小的独立空间，独立进行垃圾回收而不是每次都垃圾回收整个内存

   _评价垃圾回收算法的好坏_：吞吐量(throughput) & 停顿时间(pause times)
   吞吐量越大，停顿时间越小的算法越优

3. jvm 如何知道那些对象需要回收 ？

- 引用计数法：  
  每个对象上都有一个引用计数，对象每被引用一次，引用计数器就+1，对象引用被释放，引用计数器-1，直到对象的引用计数为 0，对象就标识可以回收  
  但是这个算法有明显的缺陷，对于循环引用的情况下，循环引用的对象就不会被回收。例如下图：对象 A，对象 B 循环引用，没有其他的对象引用 A 和 B，则 A 和 B 都不会被回收。
- root 搜索算法：  
  这种算法目前定义了几个 root，也就是这几个对象是 jvm 虚拟机不会被回收的对象，所以这些对象引用的对象都是在使用中的对象，这些对象未使用的对象就是即将要被回收的对象。简单就是说：如果对象能够达到 root，就不会被回收，如果对象不能够达到 root，就会被回收。

  以下对象会被认为是 root 对象：

  - 被启动类(bootstrap 加载器)加载的类和创建的对象
  - jvm 运行时方法区类静态变量(static)引用的对象
  - jvm 运行时方法区常量池引用的对象
  - jvm 当前运行线程中的虚拟机栈变量表引用的对象
  - 本地方法栈中(jni)引用的对象

## 参考

- oracle 文档： https://docs.oracle.com/javase/specs/jvms/se7/html/index.html
- https://zzero049.github.io/Blog/#/backend/java/java高级/JVM/01JVM概述
- 虚拟机浅谈：解释器，树遍历解释器，基于栈与基于寄存器，大杂烩 https://www.iteye.com/blog/rednaxelafx-492667
