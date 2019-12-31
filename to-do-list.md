### \0. 优秀博客、网站关注

| 网站名称                  | 网站地址                                                     | 备注 |
| ------------------------- | ------------------------------------------------------------ | ---- |
| CNCF(云原生基金会)        | https://www.cncf.io/#                                        |      |
| k8s中文社区               | https://www.kubernetes.org.cn/                               |      |
| 京东云开发者社区          | 微信公众号                                                   |      |
| 腾讯云（开发者手册-特色） | 微信公众号                                                   |      |
| 计算机科学课程学习网站    | https://github.com/ossu/computer-science/blob/dev/extras/courses.md |      |



### 1. code_basis

#### 1.1 java

> （1）自定义注解（记录时间：2019-08-25）
>
> （2）并发-线程池（记录时间：2019-08-25）

#### 1.2 go 

> （1）cobra（命令行工具）库（https://o-my-chenjian.com/2017/09/20/Using-Cobra-With-Golang/）（记录时间：2019-08-25）

### 2. arch_design

#### 2.1distributed_arch

##### 2.1.1rpc架构、通信原理、netty学习（记录时间 ：2019-08-25）

> （1）通信方式、通信协议、架构性能等优化方案；
>
> （2）同步、异步，阻塞、非阻塞，长连接、短连接；
>
> （3）rpc apply：apache thrift， gRpc，netty，dubbo，ice，**brpc-java**（百度rpc）
>
> | rpc名称       | 客户端调用方式  | 备注 |
> | ------------- | --------------- | ---- |
> | apache thrift | idl文件（.）    |      |
> | gRpc          | idl文件（.pro） |      |
> | brpc-java     | 接口类          |      |

#### 2.2 micro_svc_arch

##### 2.2.1 微服务治理

>[（1）微服务治理应用篇 – KubeSphere 灰度发布与熔断](https://www.kubernetes.org.cn/5717.html)（记录时间：2019-08-25）
>
>（2）微服务治理平台（istio、spring cloud）
>

##### 2.2.2 服务注册、服务发现、智能路由

#### 2.3 arch_design_demo

##### 2.3.1 1.3万亿条数据查询如何做到毫秒级响应？

> TiDB https://pingcap.com/docs-cn/
>
> [https://itindex.net/detail/60012-%E5%A4%A7%E6%95%B0%E6%8D%AE-%E7%9F%A5%E4%B9%8E-%E4%B8%87%E4%BA%BF](https://itindex.net/detail/60012-大数据-知乎-万亿)



### 3. tech_extension

#### 3.1 data_science

##### 3.1.1 data_analysis

>（1）混查引擎：quicksql（记录时间：2019-08-27）
>

##### 3.1.2 数据资产

​	未来公司的核心资产变为数据资产： 

（1）怎么保护我们的数据资产 - 数据安全；

（2）知道我们拥有哪些数据资产 - 数据资产管理；

（3）如何跨部门共享数据资产 - 数据分享 （实践 ：数据服务）

### 4. cloud_native

> 云原生技术栈：
>
> - 微服务
> - 容器
> - Serverless
> - Service Mesh
> - 不可变基础设施
> - 声明式API
> - ......

#### 4.1 docker

#### 4.2 k8s

##### 4.2.1 k8s云平台部署工具

>（1）kubesphere（集群部署 + 服务网格 + 服务治理 = 分布式容器管理平台）
>
>​		  https://kubesphere.io/docs/advanced-v2.0/zh-CN/installation/upgrade/
>
>（2）kubespray（集群部署）
>
>（3）rancher（集群部署 + 集群管理）https://www.rancher.cn/
>
>（4）kind（集群部署）https://kind.sigs.k8s.io/docs/contributing/project-scope/
>
>（5）rancher k3s（集群部署-边缘计算）
>
>（6）k8s studio（k8s dashboard）

##### 4.2.2 CI/CD

>（1）CDF（持续交互基金会）https://cd.foundation/about/

##### 4.2.3 网络

>1、pod带宽限制：
>
>（1）官方方案：
>
>[https://www.xiayinchang.top/2019/01/13/%E5%85%B3%E4%BA%8EK8S%E4%B8%ADPod%E5%B8%A6%E5%AE%BD%E9%99%90%E5%88%B6%E7%9A%84%E9%97%AE%E9%A2%98/](https://www.xiayinchang.top/2019/01/13/关于K8S中Pod带宽限制的问题/)
>
>https://zhuanlan.zhihu.com/p/54988169
>
>（2）阿里云提供方案：
>
>（3）带宽测试工具：
>
>https://docs.azure.cn/zh-cn/articles/azure-operations-guide/virtual-network/aog-virtual-network-iperf-bandwidth-test
>
>kubenet的网络带宽限制其实是通过tc来实现的
>
>```shell
># setup qdisc (only once)
>tc qdisc add dev cbr0 root handle 1: htb default 30
># download rate
>tc class add dev cbr0 parent 1: classid 1:2 htb rate 3Mbit
>tc filter add dev cbr0 protocol ip parent 1:0 prio 1 u32 match ip dst 10.1.0.3/32 flowid 1:2
># upload rate
>tc class add dev cbr0 parent 1: classid 1:3 htb rate 4Mbit
>tc filter add dev cbr0 protocol ip parent 1:0 prio 1 u32 match ip src 10.1.0.3/32 flowid 1:3
>```
>
>（4）linux tc详解

>https://tonydeng.github.io/sdn-handbook/linux/tc.html
>
>
>
>



#### 4.3 Service Mesh（服务网格）

（SMI）https://smi-spec.io/

##### 4.3.1 Istio

>1、参考资料：
>
>​	https://mp.weixin.qq.com/s/0nHCTQKEWGd0v9P1jtB_4A （Istio详细介绍）

##### 4.3.2 maesh（ [containous](https://github.com/containous)）

#### 4.4 （Serverless）函数服务

### 5. 文档编写

#### 5.1 画图工具

| 名称         | 地址                                   | 备注 |
| ------------ | -------------------------------------- | ---- |
| visio-时序图 | http://www.woshipm.com/ucd/607593.html |      |
|              |                                        |      |
|              |                                        |      |

### 6. linux_related

#### 6.1 命令集合

https://wangchujiang.com/linux-command/c/dig.html

>1. dig, host, nslookup 系列
>
>   https://www.cnblogs.com/sparkdev/p/7777871.html
>
>2. 
>
>