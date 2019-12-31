### 1. 整体知识架构

#### 1.1 架构图

![1570156427880](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\1570156427880.png)

​																		**图1-1 JVM 整体架构图**



<img src="https://raw.githubusercontent.com/ityouknow/diagram/master/ppt/jvm/JVM.jpg" alt="img" style="zoom:200%;" />



​																		 **图1-2 JVM参数详解**

![1570159211337](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\1570159211337.png)

​																		**图1-3 JVM 工作流程图**



![1570172222032](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\1570172222032.png)

​																**图1-4 JVM-Java应用与JVM的关系** 





#### 1.2 优秀博文

| 博文地址                              | 博文简介                  | 备注           |
| ------------------------------------- | ------------------------- | -------------- |
| https://zhuanlan.zhihu.com/p/34426768 | JVM优秀读书笔记，总结全面 | 本笔记灵感来源 |
|                                       |                           |                |
|                                       |                           |                |



### 2. JVM探索之旅

#### 2.1 类加载机制



#### 2.2 内存结构

##### 2.2.1 整体架构

![img](http://mmbiz.qpic.cn/mmbiz_png/PgqYrEEtEnprm5p8TE8Ogn2WfVM3YUA5R55vvKcRJC1UXBVrjuEJuLOxD6woyWpicufMicSZbZTpLrGrNrr0cmAQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> **方法区**和**堆**是所有线程共享的内存区域；而**java栈、本地方法栈和程序计数器**是运行时（Runtime）线程私有的内存区域。



![img](https://mmbiz.qpic.cn/mmbiz_png/PgqYrEEtEnoUSbbnzEiafyyQWUibOfnE3GicpdRQOuxWBrhB3Fic7MRf4z5ywT2RmCicibGibHNQEgUbsibLR1eLVRfo3A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

1. java应用启动的时候虚拟机干了啥？

   创建堆区、方法区、栈区， 将应用的类加载进来进行对象分配，

2. 

##### 2.2.2 堆（Heap）- 线程共享

##### 2.2.2 方法区（Method Area）- 线程共享

##### 2.2.2 Java栈（Java Stack）、本地方法栈（Native Method Stack）、程序计数器（Program Counter Register）- 线程私有

- 本地方法栈（Native Method Stacks）与 Java 虚拟机栈所发挥的作用是非常相似的，其区别不过是虚拟机栈为虚拟机执行 Java 方法（也就是字节码）服务，而本地方法栈则是为虚拟机使用到的 Native 方法服务。虚拟机规范中对本地方法栈中的方法使用的语言、使用方式与数据结构并没有强制规定，因此具体的虚拟机可以自由实现它。
- **Navtive 方法是 Java 通过 JNI 直接调用本地 C/C++ 库**，可以认为是 Native 方法相当于 C/C++ 暴露给 Java 的一个接口，Java 通过调用这个接口从而调用到 C/C++ 方法。当线程调用 Java 方法时，虚拟机会创建一个栈帧并压入 Java 虚拟机栈。然而当它调用的是 native 方法时，虚拟机会保持 Java 虚拟机栈不变，也不会向 Java 虚拟机栈中压入新的栈帧，虚拟机只是简单地动态连接并直接调用指定的 native 方法。

例如，使用一些旧的库，与硬件、操作系统进行交互，或者为了提高程序的性能。



![img](https://upload-images.jianshu.io/upload_images/14211474-fe4b43e1ff9a3386.png?imageMogr2/auto-orient/strip|imageView2/2/w/1003/format/webp)

#### 2.3 GC算法 - 垃圾回收

#### 2.4 GC分析 - 命令调优

##### 2.4.1 监控工具

| 工具名称  | 功能描述 | 备注 |
| --------- | -------- | ---- |
| jvisualvm |          |      |
|           |          |      |
|           |          |      |

