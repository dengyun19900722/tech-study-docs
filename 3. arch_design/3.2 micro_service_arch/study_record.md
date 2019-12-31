### 1. Springboot

#### 1.1 整体架构

##### 1.1.1 架构图

![enter image description here](http://images.gitbook.cn/b90b8060-d578-11e7-ba8d-675556ef95d9)

##### 1.1.2 关键词

##### 1.1.3 优秀博文

| 博文地址                                                     | 内容概述                 | 备注                        |
| ------------------------------------------------------------ | ------------------------ | --------------------------- |
| https://blog.csdn.net/lmy86263/article/details/71037316      | 什么是JMX                |                             |
| http://www.itmuch.com/spring-boot/actuator-prometheus-grafana/           https://www.callicoder.com/spring-boot-actuator-metrics-monitoring-dashboard-prometheus-grafana/ |                          | springboot + prometheus监控 |
| http://mp.163.com/v2/article/detail/D7SQCHGT0511FQO9.html    |                          |                             |
| http://www.ityouknow.com/springboot/2018/02/06/spring-boot-actuator.html |                          |                             |
| https://cloud.tencent.com/developer/article/1505497          | zabix 进行JVM监控（JMX） |                             |

​	Spring Boot 使用“习惯优于配置的理念”，采用包扫描和自动化配置的机制来加载依赖 Jar 中的 Spring bean，不需要任何 Xml 配置，就可以实现 Spring 的所有配置。虽然这样做能让我们的代码变得非常简洁，但是整个应用的实例创建和依赖关系等信息都被离散到了各个配置类的注解上，这使得我们分析整个应用中资源和实例的各种关系变得非常的困难。

#### 1.2 核心技术

##### 1.2.1 监控

![img](http://crawl.ws.126.net/nbotreplaceimg/5d0496058db2f39d0278663121dad630/c4cedc21006b5fbce308f8786a3ffe84.jpg)