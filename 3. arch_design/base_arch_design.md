# **1.** **主流架构学习**

https://lg1024.com/post/learning.html（架构师图谱）

## **1.1.** **集群架构与分布式架构比较**

### **1.1.1.** **优缺点分析**

1. #### 集群架构

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml11920\wps1.jpg) 

图1-1 集群架构图

（1）缺点：

\- 耦合度太高；

\- 增减了团队的合作成本；

\- 不能够灵活的部署；

2. #### 分布式架构

 ![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml11920\wps2.jpg)

图1-2 分布式架构图

（1）优点

\- 项目按业务拆分为多个模块，耦合度降低；

\- 单点运行，团队合作效率提高；

\- 可以灵活部署；

（2）缺点

\- 需要额外去开发让各个模块之间能够通信；

 

## **1.2.** **应用架构演进**

### **1.2.1.** **架构设计keyword**

#### **1.2.1.1.** **伸缩性**

#### **1.2.1.2.** **可用性**

#### **1.2.1.3.** **可维护性**

 

 

### **1.2.2.** **传统垂直应用架构**

#### **1.2.2.1.** **架构图**

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml11920\wps3.png)

图1-2 垂直应用架构

#### **1.2.2.2.** **伸缩方案**

**1.** **热双机（主备）方案**

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml11920\wps4.jpg) 

服务端监听浮动IP，通过watch dog监测应用进程，判断应用进程宕机后，将应用切换到备机中；

**2.** **集群部署方案**

高并发、大流量的场景则需要使用集群，使用nginx等负载均衡器做七层负载均衡，应用做对等集群部署；

#### **1.2.2.3.** **优、缺点**

**1.** **优点**

（1）技术比较单一 => 学习成本低、开发上手快；

（2）测试、部署、运维简单（单体部署）；

**3.** **缺点**

（1）

### **1.2.3.** **RPC架构**

#### **1.2.3.1.** **RPC架构原理**



##### **1.2.3.1.1.** **通信方式**

https://medium.com/@natemurthy/rest-rpc-and-brokered-messaging-b775aeb0db3

###### 1. REST

###### 2. RPC

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20151029104932230)

###### 3. Brokered Messaging

##### **1.2.3.1.2.** **通信协议（二进制传输）**

![img](https://img-blog.csdn.net/20170317150321773?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemhzaHVsaW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

​                                                                            TCP/IP五层网络模型 

###### **1.** **HTTP协议**（应用层协议）

​	Rest使用HTTP协议；

HTTP请求方法名称
--------------------------------- -------------
POST my-bucket.s3.amazonaws.com CreateObject 
GET my-bucket.s3.amazonaws.com ReadObject 
PUT my-bucket.s3.amazonaws.com UpdateObject 
DELETE my-bucket.s3.amazonaws.com DeleteObject

###### **2.** **Socket协议**

   

######  3. TCP / UDP协议

### **1.2.4** **SOA架构**

### **1.2.5** **分布式（微服务）架构**

### 1.2.6 云原生架构

 

# **2.** **架构设计案例**

## **2.1.** **设计一个秒杀系统？**

https://github.com/sunshineshu/-How-to-design-a-spike-system/blob/master/01-she-ji-miao-sha-xi-tong-shi-ying-gai-zhu-yi-de-5-ge-jia-gou-yuan-ze.md

### **2.1.1.** **关键词**

大流量高并发、动静分离

 

秒杀其实主要解决两个问题，一个是并发读，一个是并发写，并发读的核心优化理念是尽量减少用户到服务端来“读”数据，或者让他们读更少的数据；并发写的处理原则也一样，它要求我们在数据库层面独立出来一个库，做特殊的处理。