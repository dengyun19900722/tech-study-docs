# 0 参考资料

| 资料网址                                                     | 资料说明                                               | 备注 |
| ------------------------------------------------------------ | ------------------------------------------------------ | ---- |
| https://github.com/chenryn/aiops-handbook                    | AIOps手册                                              |      |
| https://github.com/logpai/awesome-log-analysis               | 专精在日志智能分析领域的论文和研究者名录收集！！！！！ | 优秀 |
| ”智能运维“公众号                                             |                                                        |      |
| https://netman.aiops.org/courses/advanced-network-management-fall2019-syllabus-2/ | AIOps课程 - 裴丹                                       |      |



# 1 无人运维评级

> - 目标：量化指标评估运维生产力，与SLA同样重要；
>
> - cores per po（CPO）：每个op（40小时/周）平均管理的x86 CPU核数；
>
> - 假设：1、各机构都需依赖已有运维技术+人力 满足一定的稳定性、可靠性SLA
>
>   ​            2、为达到同等SLA：人力越多，依赖人力的决策越多；
>
> - 与如下因素脱钩：行业、业务、规模、架构、技术、加班/兼职程度；
>
> - 服务器、存储、网络、中间件、数据库、应用运维人力都计算在内；
>
> - 计入人力运行脚本、盯屏、查看监控数据、处理报警、排除故障、运维规划、值班闲置所用工时；
>
> - 不计入开发运维工具所用工时；

| Level=【Log(CPU/100)】 | Cores Per Op（CPO） | 典型行业                     |
| ---------------------- | ------------------- | ---------------------------- |
| Level 0                | 几百                | 传统行业                     |
| Level 1                | 几千                | 基于公有云的中小型互联网公司 |
| Level 2                | 几万                | 大型互联网公司               |
| Level 3                | 几十万              |                              |
| Level 4                | 几百万              |                              |
| Level 5                | 几千万              |                              |



# 2 基于AIOps的无人运维

## 2.1 实现无人运维：基于AIOps建立运维大脑

![image-20200709211546840](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709211546840.png)



## 2.2 AIOps大脑

![image-20200709211749740](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709211749740.png)

## 2.3 AIOps架构

![image-20200709211955989](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709211955989.png)





# 3 AIOps算法进展举例

## 3.1 决策算法

### 3.1.1 AIOps算法举例

![image-20200709212110993](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709212110993.png)



![image-20200709212349007](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709212349007.png)

![image-20200709213852396](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709213852396.png)



### 3.1.2 应用场景

![image-20200709212635799](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709212635799.png)



![image-20200709212603063](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709212603063.png)



![image-20200709212743977](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709212743977.png)



![image-20200709212843647](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709212843647.png)



![image-20200709212917295](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709212917295.png)



![image-20200709212944130](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709212944130.png)





## 3.2 知识图谱

![image-20200709214128344](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709214128344.png)



![image-20200709214654579](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709214654579.png)



![image-20200709214902458](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709214902458.png)



![image-20200709214934507](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709214934507.png)



![image-20200709215229713](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709215229713.png)

![image-20200709215314313](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709215314313.png)

![image-20200709215344862](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200709215344862.png)