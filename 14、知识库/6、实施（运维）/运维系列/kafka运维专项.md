# 查看kafka消费者组

kafka

https://www.cnblogs.com/singleYao/p/16419476.html?share_token=77a87b40-e118-4899-bf37-c2a3c039ee9d

查询消费者组详情

```shell
#查看 kafka 消费者组列表：
/bin/kafka-consumer-groups.sh --bootstrap-server <kafka-ip>:9092 --list

#查看 kafka 中某一个消费者组的消费情况：
bin/kafka-consumer-groups.sh --bootstrap-server <kafka-ip>:9092 --describe --group groupname
```

![img](https://img2022.cnblogs.com/blog/1020107/202206/1020107-20220628150128936-1523382062.png)

消费积压情况分析

- LogEndOffset ：下一条将要被加入到日志的消息的位移

- CurrentOffset ：当前消费的位移

- LAG ：消息堆积量

- 消息堆积量：消息中间件服务端中所留存的消息与消费掉的消息之间的差值即为消息堆积量也称之为消费滞后量





