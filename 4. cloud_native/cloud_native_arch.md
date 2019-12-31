###  1. 整体架构

#### 1.1 架构图

#### 1.2 优秀博文

| 博文地址                                                     | 内容概述                                   | 备注 |
| ------------------------------------------------------------ | ------------------------------------------ | ---- |
| https://www.infoq.cn/article/fA42rfjV*dYGAvRANFqE            | 蚂蚁金服中间件与容器团队 -- 云原生应用解读 |      |
| https://jimmysong.io/migrating-to-cloud-native-application-architectures/ | 迁移到云原生应用架构                       |      |
| https://www.infoq.cn/article/iDkUsl3jsWk9zS9qaIgC            | 云原生生态周报                             |      |
| https://www.infoq.cn/article/xT2yhmyewicXrecS8mQI            | 云原生基石                                 |      |
| https://pivotal.io/cn/cloud-native                           | 云原生鼻祖-pivotal                         |      |
|                                                              |                                            |      |
| https://www.infoq.cn/article/ykDsBz7LFvFz-BQfs3IP            | 云计算发展历程（****）                     |      |
|                                                              |                                            |      |
| 书籍（微信读书-书架）                                        | 未来架构-从服务化到云原生                  |      |
| https://edu.aliyun.com/certification/acp01?spm=5176.11999222.1216633.4.296bff12tloGva#step=step3 | 阿里云云计算专业认证考试                   |      |



| 岗位详情                                                     | 岗位描述                           | 备注 |
| ------------------------------------------------------------ | ---------------------------------- | ---- |
| https://careers.tencent.com/jobdesc.html?postId=1173610564283273216 | 腾讯云容器高级工程师               |      |
| https://careers.tencent.com/jobdesc.html?postId=1125992326875844608 | 腾讯云serverless高级后台开发工程师 |      |
| https://careers.tencent.com/jobdesc.html?postId=1145536370823925760 | 腾讯云中间件高级后台开发工程师     |      |



### 2. 

#### 2.1 概念解读

##### 2.1.1 云计算

1、什么是云计算？

​		云计算已多特指提供各类云端服务与组件的软硬一体化***技术资源平台***，是一个带有明确商业模式的综合性载体，而大数据则是技术上处理大体量数据的方法论和实现，主要是一种技术体系——所以两者各自独立又可互相依存，比如各云计算厂商都陆续推出了云上大数据分析服务，如 AWS 的 EMR、Azure 的 HDInsight、阿里云的 E-MapReduce，本质上正是开源大数据技术在云上的实现和适配。

2、云计算的发展历程？

##### 2.1.2 云原生

![img](https://pic2.zhimg.com/80/v2-ccae214718d2510f10b03773460b8a25_hd.png)

1、什么是云原生？什么是云原生应用？

（1）云原生：

![畅谈云原生（上）：云原生应用应该是什么样子？](https://static.geekbang.org/infoq/5c7609825b419.png)

（2）云原生应用

​	 云原生是一种方法，用于构建和运行充分利用云计算模型优势的应用。

<img src="https://static.geekbang.org/infoq/5c76098114842.png" alt="畅谈云原生（上）：云原生应用应该是什么样子？" style="zoom:75%;" />

<img src="https://static001.infoq.cn/resource/image/5d/6e/5d3cf3ad2988beee45dc45f82427fd6e.png" alt="畅谈云原生（上）：云原生应用应该是什么样子？" style="zoom:80%;" />

#### 2.2 各云厂家产品介绍

##### 2.2.1 CAAS（容器服务）

| 厂家                          | 特色                                     | 备注 |
| ----------------------------- | ---------------------------------------- | ---- |
| 阿里云 - 容器服务kubernetes版 |                                          |      |
| 腾讯云 - 容器服务             | k8s集群原生概念UI化 + 应用中心、运维中心 |      |
| 京东云 - kubernetes集群       | k8s集群原生概念UI化  ~= k8s dashboard    |      |
| 金山云 - 容器引擎 (KCE)       |                                          |      |
| 百度云 - 容器引擎 CCE         | 无缝适配人工智能应用                     |      |
| Ucloud                        |                                          |      |
| 华为云                        |                                          |      |
| 平安云                        |                                          |      |
| aws                           |                                          |      |
| 微软云                        |                                          |      |

![img](https://pic4.zhimg.com/80/v2-12e258f75d5f7ceceb469e684815f30b_hd.jpg)

#### 2.3 云原生应用

##### 2.3.1 数据库

| 应用名称                         | 应用概述                                                     | 备注                                                 |
| -------------------------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| TiDB                             | 在线事务处理/在线分析处理（ HTAP: Hybrid Transactional/Analytical Processing）的融合型数据库产品 | 结合了传统的 RDBMS 和 NoSQL 的最佳特性的分布式数据库 |
| Apache ShardingSphere(Incubator) | 分布式数据库中间件解决方案组成的生态圈                       |                                                      |
| Delta Lake（数据湖) - databricks | Delta Lake 是一个存储层，为[ Apache Spark ](https://spark.apache.org/)和其他大数据引擎提供可伸缩的 ACID 事务，让用户可以基于 HDFS 和云存储构建可靠的数据湖。 | https://www.infoq.cn/article/8V7UcWWhXXeCg6xOsKvQ    |

##### 2.3.1 云数据库

| 应用名称         | 应用概述 | 备注 |
| ---------------- | -------- | ---- |
| 云数据库 - redis |          |      |
| 云数据库 - mysql |          |      |
|                  |          |      |

