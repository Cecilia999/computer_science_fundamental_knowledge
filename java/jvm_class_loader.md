# jvm class loader + 类加载的过程/机制

## 类加载的过程/机制

> note1: .class file/字节码 是一串二进制的字节流  
> note2: .class file/字节码 可以代表的 java 语言中的 class/interface

### 1. 什么是类加载

- 虚拟机把描述类的数据从 .class file 加载到内存，并对数据进行校验、转换、解析、和初始化最终形成可以被虚拟机直接使用的 java 类型，这就是虚拟机的类加载机制，这个过程通过类加载器子系统完成。

- 一个类加载过程包括了加载、验证、准备、解析、初始化、使用和卸载七个阶段。

- Java 语言运行期类加载的特性：**动态加载、动态连接**
  - 和那些在编译时需要进行连接工作的语言不同，在 java 语言里，类的**加载、连接、初始化**过程都是在**程序运行的期间**完成的
  - 决定了 java 可以动态扩展的语言特性

### 2. 类的加载过程详解

一个类的生命周期包括了加载、验证、准备、解析、初始化、使用和卸载七个阶段。其中类加载的过程包括了加载、验证、准备、解析、初始化五个阶段。
**除了初始化，其他的阶段开始的顺序都是固定的**

> 开始的顺序是固定的，不代表进行/完成的顺序是固定的，这些阶段都是交叉混合进行的，通常在一个阶段执行的过程中调用/激活另一个阶段

#### 类的生命周期

![alt text](../image/类的生命周期.jpg)

#### 类的加载过程

![alt text](../image/类加载的过程.jpg)

1.  加载(loading)

## 类加载器的结构

- 每一个 Java 虚拟机都有一个类加载器子系统（class loader subsystem），负责加载程序中的类型（类和接口），并赋予唯一的名字。每一个 Java 虚拟机都有一个执行引擎（execution engine）负责执行被加载类中包含的指令。
- JVM 的两种类装载器包括：启动类装载器和用户自定义类装载器，启动类装载器是 JVM 实现的一部分，用户自定义类装载器则是 Java 程序的一部分，必须是 ClassLoader 类的子类。

![Alt text](../image/class_loader.jpg)

1. Bootstrap ClassLoader

负责加载$JAVA_HOME 中 jre/lib/rt.jar 里所有的 class，由 C++实现，不是 ClassLoader 子类

2. Extension ClassLoader

负责加载 java 平台中扩展功能的一些 jar 包，包括$JAVA_HOME 中 jre/lib/\*.jar 或-Djava.ext.dirs 指定目录下的 jar 包

3. App ClassLoader

负责记载 classpath 中指定的 jar 包及目录中 class

4. Custom ClassLoader

属于应用程序根据自身需要自定义的 ClassLoader，如 tomcat、jboss 都会根据 j2ee 规范自行实现 ClassLoader 加载过程中会先检查类是否被已加载，检查顺序是自底向上，从 Custom ClassLoader 到 BootStrap ClassLoader 逐层检查，只要某个 classloader 已加载就视为已加载此类，保证此类只所有 ClassLoader 加载一次。而加载的顺序是自顶向下，也就是由上层来逐层尝试加载此类。

## 参考

- 《深入理解 java 虚拟机》 第七章-虚拟机类加载机制
- oracle 文档： https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-5.html
