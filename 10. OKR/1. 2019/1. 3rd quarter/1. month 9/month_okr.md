https://yunlzheng.gitbook.io/prometheus-book/part-ii-prometheus-jin-jie/exporter/commonly-eporter-usage/use-prometheus-monitor-container



https://github.com/1046102779/prometheus/tree/master/prometheus/querying

| 表达式（promQL）                                             | 表达式说明                      | 备注 |
| ------------------------------------------------------------ | ------------------------------- | ---- |
| sum(increase(container_network_receive_packets_total[1m]) by (pod_name) | 计算容器在1分钟内接收到的总流量 |      |
|                                                              |                                 |      |
|                                                              |                                 |      |

cadvisor集成在kubelet中；



![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20170909222324259?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaHV3aF8=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)