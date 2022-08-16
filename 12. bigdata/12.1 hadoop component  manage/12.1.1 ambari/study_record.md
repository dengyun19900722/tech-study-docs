# 1 架构

## 1.1 整体架构



![这里写图片描述](https://img-blog.csdn.net/20170310103608145?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmd5dXFpYW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

​                                                               **图1-1 ambari 整体架构**

## 1.2 ambari server

![这里写图片描述](https://img-blog.csdn.net/20170314105536114?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmd5dXFpYW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

​                                                               **图1-2 ambari server架构**

![这里写图片描述](https://img-blog.csdn.net/20170314112026451?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmd5dXFpYW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

​                                                               **图1-3 ambari server架构 - 数据流向**



**如上图所示，server端主要维护三类状态：**

- **Live Cluster State**：集群现有状态，各个节点汇报上来的状态信息会更改该状态;
- **Desired State**：用户希望该节点所处状态，是用户在页面进行了一系列的操作，需要更改某些服务的状态，这些状态还没有在节点上产生作用;
- **Action State**：操作状态，是状态改变时的请求状态，也可以看作是一种**中间状态**，这种状态可以辅助Live Cluster State向Desired State状态转变。

​        Ambari-server的Heartbeat Handler模块用于接收各个agent的心跳请求（心跳请求里面主要包含两类信息：节点状态信息和返回的操作结果），把节点状态信息传递给FSM状态机去维护着该节点的状态，并且把返回的操作结果信息返回给Action Manager去做进一步的处理。
​       Coordinator模块又可以称为API handler，主要在接收WEB端操作请求后，会检查它是否符合要求，stage planner分解成一组操作，最后提供给Action Manager去完成执行操作。

## 1.3 ambari agent

![这里写图片描述](https://img-blog.csdn.net/20170314110616037?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmd5dXFpYW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

​											**图1-4 ambari agent架构**

### 1.3.1 采集所在节点的信息并且汇总发心跳汇报给ambari-server

![这里写图片描述](https://img-blog.csdn.net/20170314111438886?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmd5dXFpYW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

​																	**图1-5 节点信息上报流程**

### 1.3.2 处理ambari-server的执行请求

![这里写图片描述](https://img-blog.csdn.net/20170314111209212?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmd5dXFpYW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

​																	**图1-6 任务执行流程**



# 2 技术栈

## 2.1 ambari server

- 服务代码框架 - java8、spring
- 依赖注入框架 - guice
- Rest框架 - JAX-RS
- runtime tool - Jetty Server
- java安装治理框架 - Spring Security
- agent 脚本、配置管理：python
- 数据库 - Postgres、Oracle、MySQL

## 2.2 ambari  agent

- 开发框架 - python
- 监控框架 - nagios、ganglia
- 任务执行框架 - puppet、python

## 2.3 ambari  web

- mvc框架 - Ember.js
- 页面渲染引擎 - handlebars.js
- 外观框架 - bootstrap
- css处理器 - Less
- runtime tool：nodejs
- 打包框架：brunch

# 3 源码分析

## 3.1 基本概念

| 资源（实体）名称                   | 资源说明                                                     |
| ---------------------------------- | ------------------------------------------------------------ |
| Resource                           | Ambari把可以被管理的资源的抽象为一个Resource实例，资源可以包括服务、组件、主机节点等，一个resource实例中包含了一系列该资源的属性； |
| Property                           | 服务组件的指标名称                                           |
| ResourceProvider和PropertyProvider | Resource和Property的提供方，获取指标需要先获取Resource，然后获取Property对应的metric； |
| Query                              | Query是Resource的内部对象，代表了对该资源的操作；            |
| Request                            | 一个Request代表了对Resource的操作请求，包含http信息及要操作的Resource的实例，Request按照http的请求方式分为四种：GET、PUT、DELETE、POST； |
| Predicate                          | 一个Predicate代表了一系列表达式，如and、or等；               |

## 3.2  总体目录

| 目录           | 描述                                                       |
| -------------- | ---------------------------------------------------------- |
| ambari-server  | Ambari的Server程序，主要管理部署在每个节点上的管理监控程序 |
| ambari-agent   | 部署在监控节点上运行的管理监控程序                         |
| ambari-web     | Ambari页面UI的代码，作为用户与Ambari server交互的。        |
| ambari-views   | 用于扩展Ambari Web UI中的框架                              |
| ambari-common  | Ambari-server 和Ambari-agent 共用的代码                    |
| ambari-metrics | 在Ambari所管理的集群中用来收集、聚合和服务Hadoop和系统指标 |
| contrib        | 自定义第三方库                                             |
| Docs           | 文档                                                       |

## 3.2 ambari server

### 3.2.1 代码结构

![image-20200408103501578](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200408103501578.png)

| 目录                                         | 功能描述                                                     |
| -------------------------------------------- | ------------------------------------------------------------ |
| org.apache.ambari.server.api.services        | 对web接口的入口方法，处理/api/v1/*的请求；                   |
| org.apache.ambari.server.controller          | 对Ambari中cluster的管理操作，如新增host，更新service、删除Component等； |
| org.apache.ambari.server.controller.internal | 主要存放ResourceProvider和PropertyProvider；                 |
| org.apache.ambari.server.orm.*               | 主要存放ResourceProvider和PropertyProvider；<br/>org.apache.ambari.server.orm.*：数据库操作相关； |
| org.apache.ambari.server.agent.rest          | 处理与Agent的接口的入口方法；                                |
| org.apache.ambari.security                   | 使用Spring Security来做权限管理                              |



## 3.2 ambari  agent

![image-20200408105634967](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200408105634967.png)

### 3.2.3 代码结构

![这里写图片描述](https://img-blog.csdn.net/20170314101532967?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmd5dXFpYW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast

## 3.3 ambari  web

### 3.3.1 根目录

| 目录或文件    | 功能描述                                                     |
| ------------- | ------------------------------------------------------------ |
| app/          | 主要应用程序代码。包括Ember中的view、templates、controllers、models、routes |
| config.coffee | Brunch应用程序生成器的配置文件                               |
| package.json  | Npm包管理配置文件                                            |
| test/         | 测试文件                                                     |
| vendor/       | Javascript库和样式表适用第三方库                             |

### 3.3.2 **ambari-web/app/**

| 目录或文件   | 功能描述                          |
| ------------ | --------------------------------- |
| assets/      | 静态文件                          |
| controllers/ | 控制器                            |
| data/        | 数据                              |
| mappers/     | JSON数据到Client的Ember实体的映射 |
| models/      | Javascript库和样式表适用第三方库  |
| routes/      | 路由器                            |
| styles/      | 样式文件                          |
| views/       | 视图文件                          |
| templates/   | 页面模板                          |
| app.js       | Ember主程序文件                   |
| config.js    | 配置文件                          |

### 3.3.3 **ambari-web/app/templates 模版目录**

| 目录或文件             | 功能描述                   |
| ---------------------- | -------------------------- |
| common                 | 公用模板（可以不动）       |
| main                   | 模板的主体部分             |
| utils                  | 工具模板                   |
| wizard ambari          | 部署子模板                 |
| application.hbs ambari | 主体模板                   |
| experimental.hbs       | 实验性模板，用于测试新模板 |
| installer.hbs ambari   | 部署入口模板               |
| login.hbs              | 登陆模板                   |
| main.hbs               | 顶上的导航条模板           |

**eg：main目录下模板说明**

![这里写图片描述](https://img-blog.csdn.net/20170314114222725?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbmd5dXFpYW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

    --|dashboard                              Dashboard标签模板
        --|widgets                            组件模板
            --|cluster_metrics.hbs            生成显示集群资源信息的图表
            --|hbase_links.hbs                生成Hbase的监控图表
            --|hdfs_links.hbs                 生成HDFS的监控图表
            --|pie_chart.hbs                  生成显示饼状图的监控图表
            --|simple_text.hbs                生成显示简单文字的监控图表
            --|uptime.hbs                     生成集群启动信息的监控图表
            --|yarn_links.hbs                 生成Yarn的监控图表
        --|config_history.hbs                 Config History标签模板
        --|edit_widget_popup.hbs              编辑组件弹出模板
        --|plus_button_filter.hbs             按下后的反应过滤器(?)
        --|widgets.hbs                        用于生成操作和生成监控图表
    --|service                                services标签模板
    --|hosts                                  hosts标签模板
    --|alerts                                 alerts标签模板
    --|admim                                  admin标签模板
    --|charts                                 图表模板
    --|service.hbs                            services标签入口模板
    --|hosts.hbs                              hosts标签入口模板
    --|alerts .hbs                            alerts标签入口模板
    --|admin.hbs                              admin标签入口模板
    --|charts.hbs                             图表入口模板
    --|memu.hbs                               菜单栏入口模板
    --|memu_item.hbs                          菜单栏入口模板
    --|views.hbs                              生成组件列表(?)


# 4 监控

## 4.1 选型：nagios、ganglia

### 4.1.1 nagios

​		nagios 结构上来说， 可分为核心（nagios core）和插件（nagios plugins）两个部分。Nagios 的核心部分只提供了很少的监控功能，因此要搭建一个完善的 IT 监控管理系统，用户还需要在 Nagios 服务器安装相应的插件，插件可以从 Nagios 官方网站下载 http://www.nagios.org/，也可以根据实际要求自己编写所需的插件。

#### Nagios 可实现的功能特性

- 监控网络服务（SMTP、POP3、HTTP、FTP、PING 等）；
- 监控本机及远程主机资源（CPU 负荷、磁盘利用率、进程 等）；
- 允许用户编写自己的插件来监控特定的服务，方便地扩展自己服务的检测方法，支持多种开发语言（Shell、Perl、Python、PHP 等）
- 具备定义网络分层结构的能力，用"parent"主机定义来表达网络主机间的关系，这种关系可被用来发现和明晰主机宕机或不可达状态；
- 当服务或主机问题产生与解决时将告警发送给联系人（通过 EMail、短信、用户定义方式）；
- 可以支持并实现对主机的冗余监控；
- 可用 WEB 界面用于查看当前的网络状态、通知和故障历史、日志文件等；



```
https://github.com/wurstmeister/kafka-docker

https://github.com/rmohr/docker-activemq

https://www.jianshu.com/p/3e54a5a39683

jax-rs
https://www.ibm.com/developerworks/cn/java/j-lo-jaxrs/index.html
https://www.ibm.com/developerworks/cn/linux/1309_luojun_nagios/index.html
https://www.ibm.com/developerworks/cn/linux/l-ganglia-nagios-1/index.html
```



## 4.2 应用

# 5 扩展

## 5.1 基于ansible部署的组件怎么集成进来？



## 5.2 puppet VS ansible？

**1、服务器端：**
puppet：至少包含一个或多个puppetmaster服务器，每个客户端安装agent包。
ansible：不需要master和agent，只需要一个节点列表（inventory），允许使用SSH，就可以连接各个节点。

**2、拉取/推送模式（pull/push）：**
puppet：客户端会定期向服务器端确认，接收或者”拉取”需要被应用的配置。
ansible：通过ssh协议将命令发送到远程主机，客户端除了python以外不需要安装其他东西。

**3、模块：**
puppet：使用一些比较基本的组件(资源、类、定义、文件、模板等)自己组合成模块。
ansible：在安装时，包含了扩展的自动化模块。

**4、使用语言：**
puppet：基于Ruby搭建，语法格式采用基于Ruby的DSL语言（puppet自己的语言），template模板采用Ruby的ERB。
ansible：基于python搭建，语法格式采用YAML格式，模板采用Jinja2语言。

**5、DevOps工具支持：**
都非常好的支持开发运维工具，比如Vagrant, Packer, and Jenkins。

**6、依赖关系：**
puppet：puppet 的manifest中定义的资源在执行时，不是按照顺序依次执行的，是按照任意顺序执行的，除非明确使用了before、require等关键字或者定义依赖关系。
ansible：ansible的playbook按照定义的顺序，依次执行。