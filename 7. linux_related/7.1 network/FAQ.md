### 1. 服务器重装系统后，网络不通，怎么排查？

#### 1.1 排查步骤

##### 1.1.1 步骤1： 检查所有网卡的物理连通性

（1）启动所有网卡（使用任意错误IP）

```shell
sudo ifconfig ethxxxx 192.192.185.55/24 up
```

![1567654368622](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\1567654368622.png)



（2）使用ethtool工具检查网卡物理连通性，如下图所示

```shell
sudo ethtool ethxxxxx(网卡名称)
```



<img src="C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\1567654380346.png" style="zoom:50%;" />

##### 1.1.2 步骤2： 测试处于联通状态的网卡的网络连通

​	给步骤1测试物理连通（网线已插）的网卡配置正确的IP，测试网络是否联通；

```shell
ping gatewayip
```

##### 1.1.3步骤3： 配置好网卡后，循环探测同网段ip



