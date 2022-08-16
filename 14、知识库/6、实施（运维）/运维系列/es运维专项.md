# 1运维场景

## 1.1 基础知识

- **es的索引，文档，节点，分片，副本分别是什么意思？**

  - 索引和文档，是偏向**开发人员视角**，逻辑概念

  - 节点，分片，副本等，**运维人员**可能会偏重点，偏向物理概念。

- **目前项目策略**
- 主分片数1，副本数为2

### 1.1.1 文档（doc）

​		文档(document)：是ES 所有可搜索数据的最小单位，它会被[序列化](https://so.csdn.net/so/search?q=序列化&spm=1001.2101.3001.7020)成JSON格式(可以包含 不同的类型的字段)，保存到ES中。每个文档都有一个UID，可以自己定义，也可以交给系统生成。

- 元数据：用于标注 文档的相关信息

- 字段：类似于 [关系型数据库](https://so.csdn.net/so/search?q=关系型数据库&spm=1001.2101.3001.7020)的字段，支持数据/嵌套

  **注：doc类比关系型数据库（如：oracle）的数据行（记录），1个doc就是1条数据**

![img](https://img-blog.csdnimg.cn/20210407104312838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nNTMyNDQ5Ng==,size_16,color_FFFFFF,t_70)

### 1.1.2 索引（index）

​		索引(index)：是文档的容器，是**一类文档的组合**。索引体现了 逻辑空间的概念，它有自己的定义**(mapping和setting)**，

mapping定义了包含文档的字段和字段类型。**setting**定义不同数据的分布，有多少分片，有多少副本，怎么分布。

​		**注：index类比关系型数据库（如：oracle）的表，mapping、setting类比于表结构**

**es类比关系型数据库**

| Elasticsearch | 关系型数据库 |
| ------------- | ------------ |
| index         | table        |
| document      | row          |
| field         | colum        |
| mapping       | schema       |

![img](https://img-blog.csdnimg.cn/20210407105229217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nNTMyNDQ5Ng==,size_16,color_FFFFFF,t_70)



![img](https://img-blog.csdnimg.cn/20210407105335710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nNTMyNDQ5Ng==,size_16,color_FFFFFF,t_70)





### 1.1.3 节点（node）

​		是ES 的实例，本质上是一个java 进程。每个节点在启动后，会分配一个UID，保存在 data 目录下。每个节点都保存了集群的状态信息。

- **Master角色节点**：只有Master节点能够修改集群状态信息(包括 所有节点的信息，所有的索引mapping和setting信息，分片的路由信息)，具体负责集群相关的操作，例如创建或删除索引，跟踪哪些节点是集群的一部分，以及决定将哪些分片分配给哪些节点。集群必须有这个角色节点
- **Client角色节点**：负责处理路由请求，处理搜索，分发索引操作，集群必须有这个角色节点
- **Data角色节点**：**负责保存数据**。集群必须有这个角色节点
- **Coordinating功能节点**： 负责接收client的请求，将请求分发到合适的节点，最后把结果汇聚到一起。每个节点默认都是Coordinating节点

### 1.1.4 模板（template）

​		https://www.cnblogs.com/shoufeng/p/10641560.html

### 1.1.4 映射（mapping）

### 1.1.3 es查询场景

#### 1.1.3.1 全文检索（日志类）

1、match：根据字段进行匹配。select * from table where name = '';

2、match_all：无条件匹配。select * from table;

3、multi_match：从指定“字段中匹配。select * from table where name=shouji and age=shouji;

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
1 GET product/_search
2 {
3   "query": {
4     "multi_match": {
5       "query": "shouji",
6       "fields": ["name","age"]
7     }
8   }
9 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

4、match_phrase：会被分词

　　被检索字段必须包含match_phrase中的所有词项并且顺序必须相同

　　被检索字段包含的match_phrase中的词项之间不能有其他问题

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
1 GET product/_search
2 {
3   "query": {
4     "match_phrase": {
5       "name": "my name"
6     }
7   }
8 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

如上代码：查询的name字段必须为my name，不能为my xx name、name my

#### 1.1.3.2 精确查询（指标类）

term：搜索词不会被分词

1、如下图查询没有结果是因为：搜索词没有被分词，所以my name lyc被当成一个整体。但是索引中my name lyc被分成了my、name、lyc。所有没有匹配记录

![img](https://img2022.cnblogs.com/blog/1805944/202202/1805944-20220210104823628-2027741386.png)

 2、如下图就有结果，因为搜索词没有被分词，而索引中name字段取了keyword，也不会被分词。所以有匹配结果

![img](https://img2022.cnblogs.com/blog/1805944/202202/1805944-20220210105025885-828025402.png)

 3、terms

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 GET product/_search
 2 {
 3   "query": {
 4     "terms": {
 5       "tags": [
 6         "lowbee",
 7         "gongjiaoka"
 8       ]
 9     }
10   }
11 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

4、range：范围查询

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 GET product/_search
 2 {
 3   "query": {
 4     "range": {
 5       "price": {
 6         "gte": 10,
 7         "lte": 20
 8       }
 9     }
10   }
11 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

#### 1.1.3.3 过滤器

作用：筛选数据，不会计算相关度评分，提高效率

　　　而query则是会计算相关度评分的

若数据量非常大的时候使用filter会降低性能开销

#### 1.1.3.4 组合查询（bool query）

1、must：所有条件都必须符合相当于sql中的and

使用下面代码查询出的结果有相关度评分

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 GET product/_search
 2 {
 3   "query": {
 4     "bool": {
 5       "must": [
 6         {
 7           "match": {
 8             "name": "lyc"
 9           }
10         }
11       ]
12     }
13   }
14 }   
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

2、filter：将上图中must替换为filter。替换后查出的结果相关度评分为0。

3、must_not：所有条件都不符合的结果

4、should：或者，相当于sql中的or

5、must与filter组合使用

下面代码为must与filter组合使用，若样本很多时，可以先使用filter进行过滤(filter不计算相关度分数，节省了性能消耗)，之后再使用match计算相关度评分。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 GET product/_search
 2 {
 3   "query": {
 4     "bool": {
 5       "must": [
 6         {
 7           "match": {
 8             "name": "lyc"
 9           }
10         }
11       ],
12       "filter": [
13         {
14           "range": {
15             "age": {
16               "gte": 10
17             }
18           }
19         }
20       ]
21     }
22   }
23 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

6、should与filter或must组合使用时

　　minimum_should_match：若没有这个参数，should条件可以被忽略(不匹配)。后面的数字表示需要有几个should满足条件(若组合查询有must或者filter时该参数默认为0；若只有should，该参数默认为1；若需要满足其他场景，该参数需要手动设置)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 GET product/_search
 2 {
 3   "query": {
 4     "bool": {
 5       "filter": [
 6         {
 7           "range": {
 8             "age": {
 9               "gte": 10
10             }
11           }
12         }
13       ],
14       "should": [
15         {
16           "match_phrase": {
17             "name": "my name"
18           }
19         }
20       ],
21       "minimum_should_match": 1  
22     }
23   }
24 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

### 1.1.3 es核心api

#### 1.1.3.1 api清单

- **集群管理API（GET _cluster/）**：https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-allocation-explain.html
- **_cat api（GET _cat/）**：https://www.elastic.co/guide/en/elasticsearch/reference/current/cat.html#cat

##### 1.1.3.1.1 _cluster api



##### 1.1.3.1.2 _cat api

```shell
GET _cat/                    #列出所有_cat命令
 
# 1、磁盘还有数据调配信息
/_cat/allocation             #查看单节点的shard分配整体情况
/_cat/shards                 #查看各shard的详细情况
/_cat/shards/{index}         #查看指定分片的详细情况
/_cat/master                 #查看master节点信息
/_cat/nodes                  #查看所有节点信息
/_cat/indices                #查看集群中所有index的详细信息
/_cat/indices/{index}        #查看集群中指定index的详细信息
/_cat/segments               #查看各index的segment详细信息,包括segment名, 所属shard, 内存(磁盘)占用大小, 是否刷盘
/_cat/segments/{index}       #查看指定index的segment详细信息
/_cat/recovery               #查看集群内每个shard的recovery过程.调整replica。
/_cat/recovery/{index}       #查看指定索引shard的recovery过程
/_cat/health                 #查看集群当前状态：红、黄、绿
/_cat/pending_tasks          #查看当前集群的pending task

# 2、集群或者索引文档数量 
/_cat/count                  #查看当前集群的doc数量
/_cat/count/{index}          #查看指定索引的doc数量

# 3、索引别名
/_cat/aliases                #查看集群中所有alias信息,路由配置等
/_cat/aliases/{alias}        #查看指定索引的alias信息

# 4、插件列表
/_cat/plugins                #查看集群各个节点上的plugin信息
/_cat/fielddata?s=size:desc  #查看当前集群各个节点的fielddata内存使用情况(按大小倒序)
/_cat/fielddata/{fields}     #查看指定field的内存使用情况,里面传field属性对应的值

# 5、获取node属性信息
/_cat/nodeattrs              #查看单节点的自定义属性
/_cat/repositories           #输出集群中注册快照存储库
/_cat/templates              #输出当前正在存在的模板信息
```



#### 1.1.3.1 查看集群配置（磁盘水位）

```shell
# 1、查看集群配置，找到磁盘水位配置
curl -XGET "localhost:9200/_cluster/settings"

# 2、修改磁盘水位限制值
curl -XPUT "localhost:9200/_cluster/settings" -H 'Content-Type: application/json' -d'
{
  "transient": {
    "cluster.routing.allocation.disk.watermark.low": "90%"
  }
}'
```

#### 1.1.3.2 查询未分派的分片（unassigned shard）

```bash
#1、查询所有未分派的分片
curl -XGET localhost:9200/_cat/shards?h=index,shard,prirep,state,unassigned.reason| grep UNASSIGNED

#2、查看未分派分片的详细异常信息
curl -XGET localhost:9200/_cluster/allocation/explain?pretty

```

![img](https://main.qcloudimg.com/raw/bdb53f699cc543c72d0dbeab86433937.png)







## 1.2 常见故障运维

### 1.2.1 集群健康状态异常（RED、YELLOW）如何解决？

#### 1.2.1.1 集群状态为什么会异常？

集群状态在什么情况下发生 RED 和 YELLOW：

- 当集群存在未分配的**主索引分片**，集群状态会为 RED。该情况影响索引读写，需要重点关注。
- 当集群所有主索引分片都是已分配的，但是存在未分配的**副本索引分片**，集群状态则会 YELLOW。该情况不影响索引读写，一般会自动恢复。

#### 1.2.1.2 查看集群状态（发现问题）

使用 kibana 开发工具（DevTool），查看集群状态：

```shell
GET /_cluster/health
```

这里可以看到，当前集群状态为 red，有9个未分配的分片。
![img](https://main.qcloudimg.com/raw/611dc9996eefdbdef8e11c90cf139297.png)

**ES 健康接口返回内容官方解释：**

| 指标                             | 含义                                                         |
| :------------------------------- | :----------------------------------------------------------- |
| cluster_name                     | 集群的名称                                                   |
| status                           | 集群的运行状况，基于其主要和副本分片的状态。状态为： – green：所有分片均已分配 – yellow：所有主分片均已分配，但未分配一个或多个副本分片。如果群集中的某个节点发生故障，则在修复该节点之前，某些数据可能不可用 – **red：未分配一个或多个主分片，因此某些数据不可用**。在集群启动期间，这可能会短暂发生，因为已分配了主要分片 |
| timed_out                        | 如果 false 响应在 timeout 参数指定的时间段内返回（30s默认情况下） |
| number_of_nodes                  | 集群中的节点数                                               |
| number_of_data_nodes             | 作为专用数据节点的节点数                                     |
| active_primary_shards            | 活动主分区的数量                                             |
| active_shards                    | 活动主分区和副本分区的总数                                   |
| relocating_shards                | 正在重定位的分片的数量                                       |
| initializing_shards              | 正在初始化的分片数                                           |
| **unassigned_shards**            | **未分配的分片数（重点关注项）**                             |
| delayed_unassigned_shards        | 其分配因超时设置而延迟的分片数                               |
| number_of_pending_tasks          | 尚未执行的集群级别更改的数量                                 |
| number_of_in_flight_fetch        | 未完成的访存数量                                             |
| task_max_waiting_in_queue_millis | 自最早的初始化任务等待执行以来的时间（以毫秒为单位）         |
| active_shards_percent_as_number  | 集群中活动碎片的比率，以百分比表示                           |

#### 1.2.1.3 问题分析（分析问题）

当集群状态异常时，需要重点关注 unassigned_shards 没有正常分配的分片，这里举例说明其中一种场景。

##### 1.2.1.3.1 找到异常索引

查看索引情况，并根据返回找到状态异常的索引。

```
GET /_cat/indices
```

![img](https://main.qcloudimg.com/raw/3e6d4efed0f7e807292af5c955178aac.png)



##### 1.2.1.3.2 查看详细的异常信息



```
GET /_cluster/allocation/explain
```

![img](https://main.qcloudimg.com/raw/bdb53f699cc543c72d0dbeab86433937.png)



这里通过异常信息可以看出：

1. 主分片当前处于未分配状态（`current_state`），发生这个问题的原因是因为分配了该分片的节点已从集群中离开（`unassigned_info.reason`）。
2. 上述问题发生后，分片无法自动分配分片的原因是集群中没有该分片的可用副本（`can_allocate`）。
3. 同时也给出了更详细的信息（`allocate_explanation`）。

这种情况发生的原因是因为集群有节点下线，导致主分片已没有任何可用的分片数据，当前唯一能做的事就是等待节点恢复并重新加入集群。

> 注意：
>
> 某些极端场景，例如单副本集群的分片发生了损坏，或是文件系统故障导致该节点被永久移除，而此时只能接受数据丢失的事实，并通过 [reroute commends](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-reroute.html) 来重新分配空的主分片。为了尽量避免这种极端的场景，建议合理设计索引分片，不要给索引设置单副本。这里所谓的单副本，指的是索引有主分片，但没有副本分片，或称之为0副本。合理设计索引分片，可以将集群的总分片控制在一个很健康的规模，可以在保证高可用的情况下更加充分地利用集群分布式的特性，提高集群整体性能。

##### 1.2.1.3.3 分片未分配（`unassigned_info.reason`）的所有可能

可通过如下分析方式初步判断集群产生未分配分片的原因，一般都可以在 allocation explain api 中得到想要的答案。

> 说明：
>
> 集群状态如果长时间未自动恢复，或是无法解决，则需要通过 [售后支持](https://cloud.tencent.com/online-service?from=connect-us) 联系腾讯云技术支持。

| reason                  | 原因                                  |
| :---------------------- | :------------------------------------ |
| INDEX_CREATED           | 索引创建，由于 API 创建索引而未分配的 |
| CLUSTER_RECOVERED       | 集群恢复，由于整个集群恢复而未分配    |
| INDEX_REOPENED          | 索引重新打开                          |
| DANGLING_INDEX_IMPORTED | 导入危险的索引                        |
| NEW_INDEX_RESTORED      | 重新恢复一个新索引                    |
| EXISTING_INDEX_RESTORED | 重新恢复一个已关闭的索引              |
| REPLICA_ADDED           | 添加副本                              |
| ALLOCATION_FAILED       | 分配分片失败                          |
| **NODE_LEFT**           | **集群中节点丢失（重要关注项）**      |
| REROUTE_CANCELLED       | reroute 命令取消                      |
| REINITIALIZED           | 重新初始化                            |
| REALLOCATED_REPLICA     | 重新分配副本                          |

#### 1.2.1.3 解决方案（解决问题）

##### 1.2.1.3.1 现场处理案例（温州）

- 集群策略：3台物理机模式，节点分布：**1（3data节点）、2（3data节点）、3（2协调节点、2master节点）**
- 问题描述：es入库慢（200条/60s），集群状态Red
- 问题分析：7月的所有索引（index）状态都为Red，未分配一个或多个主分片
- 解决方案：排查得知，节点2上的数据节点全部挂掉（内存溢出，配置为4g），通过调大数据节点内存为8g，重启数据节点，问题修复



### 1.2.2 集群磁盘使用率高和 read_only 状态问题如何解决？

#### 1.2.2.1 问题现象（发现问题）

​		当磁盘使用率超过85%，或者达到100%，会导致 Elasticsearch 集群或 Kibana 无法正常提供服务，可能会出现以下几种问题场景：

- 在进行索引请求时，返回类似 **`{[FORBIDDEN/12/index read-only/allow delete(api)];","type":"cluster_block_exception"}`** 的报错。

- 在对集群进行操作时，返回类似 **`[FORBIDDEN/13/cluster read-only / allow delete (api)]`** 的报错。

- 集群处于 Red 状态，严重情况下存在节点未加入集群的情况（可通过 `GET _cat/allocation?v` 命令查看），并且存在未分配的分片（可通过 `GET _cat/allocation?v` 命令查看）。

- 通过 Elasticsearch 控制台的**节点监控**页面，集群节点磁盘使用率曾达到或者接近100%。

  > 注意：
  >
  > 磁盘满可能会出现登录失败，导致无法手动清理数据，因此建议配置告警策略，及时清理数据。

#### 1.2.2.2 问题分析（分析问题）

​		上述问题是由于磁盘使用率过高所导致。数据节点的磁盘使用率存在以下三个水位线，超过水位线可能会影响 Elasticsearch 或 Kibana 服务。

- 当集群磁盘使用率超过85%：会导致新的分片无法分配。
- 当集群磁盘使用率超过90%：Elasticsearch 会尝试将对应节点中的分片迁移到其他磁盘使用率比较低的数据节点中。
- 当集群磁盘使用率超过95%：系统会对 Elasticsearch 集群中对应节点里每个索引强制设置 read_only_allow_delete 属性，此时该节点上的所有索引将无法写入数据，只能读取和删除对应索引。

#### 1.2.2.3 解决方案（解决问题）

##### 1.2.2.3.1 清理集群过期数据

1. 用户可以通过访问【Kibana】>【Dev Tools】删除过期索引释放磁盘空间。步骤如下：

   > 警告：
   >
   > 数据删除后将无法恢复，请谨慎操作。您也可以选择保留数据，但需进行磁盘扩容。

   第一步：开启集群索引批量操作权限。

   ```
   PUT _cluster/settings
    {
               "persistent": {
                "action.destructive_requires_name": "false"
               }
   }
   ```

   第二步：删除数据，例如

   ```
   DELETE NginxLog-12*
   ```

   ```
   DELETE index-name-*
   ```

2. **执行完上述步骤后，还需要在 Kibana 界面的【Dev Tools】中执行如下命令：**

   - **关闭索引只读状态，执行如下命令：**

     ```shell
     PUT _all/_settings
     {
                 "index.blocks.read_only_allow_delete": null
     }
     ```

   - **关闭集群只读状态，执行如下命令：**

     ```shell
     PUT _cluster/settings
     {
                 "persistent": {
                     "cluster.blocks.read_only_allow_delete": null
                 }
     }
     ```

3. 查看集群索引是否依然为`read_only`状态，索引写入是否恢复正常。

4. 若集群是否依然为 Red 状态，执行以下命令，查看集群中是否存在未分配的分片。

   ```
   GET /_cluster/allocation/explain
   ```

5. 等待分片下发完成后，查看集群状态。如果集群状态依然为 Red，请通过 [售后支持](https://cloud.tencent.com/online-service?from=connect-us) 联系腾讯云技术支持。

6. 为避免磁盘使用率过高影响 Elasticsearch 服务，建议开启磁盘使用率监控报警，及时查收报警短信，提前做好防御措施。

##### 扩容es数据存储节点空间

1. 完成磁盘扩容操作；

2. 在 Kibana 界面的【Dev Tools】中执行如下命令：

   - 关闭索引只读状态，执行如下命令：

     ```
     PUT _all/_settings
       {
                 "index.blocks.read_only_allow_delete": null
       }
     ```

   - 关闭集群只读状态，执行如下命令：

     ```
     PUT _cluster/settings
     {
                 "persistent": {
                 "cluster.blocks.read_only_allow_delete": null
                 }
     }
     ```



### 1.2.3 集群整体 CPU 使用率过高问题如何解决？



- 


#### 问题解决方案（unassigned shards）

1. 停止指标采集程序（docker stop）；

2. 删除7月份的问题索引；

   ```shell
   curl -XDELETE  http://esMasterNodeIp:19201/indexName
   ```

3. 重启data-center模块，重建索引；**（项目稳定性瓶颈：data-center、采集模块、es集群策略）**

4. 修改消费者组，启动采集程序重新采集指标；

### 1.2.2 Java访问Elasticsearch报错Request cannot be executed; I/O reactor status: STOPPED 

### 问题描述

使用ES过程中遇到一个Request cannot be executed; I/O reactor status: STOPPED 的异常，大概意思是和server端的连接异常终止了。开始以为是引用的版本不对，或者自己使用问题，后来发现就是因为OOM导致程序宕机，进而引发连接终止。


