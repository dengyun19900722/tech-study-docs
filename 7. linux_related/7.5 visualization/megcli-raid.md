## 1 Proxmox

| 网站地址                                  | 网站说明                 | 备注 |
| ----------------------------------------- | ------------------------ | ---- |
| https://pve.proxmox.com/wiki/ZFS_on_Linux | proxmox 构建ZFS pool指导 |      |
| https://www.jianshu.com/p/0b4e0f5ffe93    | Megacli64使用 - 为       |      |
| https://www.xiangzhiren.com/archives/283  | ZFS存储池介绍 - Raid类型 |      |
| http://www.51niux.com/?id=77              | Megacli64常用命令        |      |



### 1.1 虚拟化操作步骤

#### 1.1.1 步骤1：连接管理口、备份网络信息

```
ip addr
```



#### 1.1.2 步骤2：挂载proxmox的iso文件重装系统

​		修改系统启动方式，读取iso文件重装系统

#### 1.1.3 步骤3：创建ZFS存储池

~~~shell
Get the parameters:    

```
# ./Megacli64 -LDInfo -LALL -aAll
Virtual Drive: 24 (Target Id: 24)
Name                :
RAID Level          : Primary-0, Secondary-0, RAID Level Qualifier-0
Size                : 1.817 TB
State               : Optimal
Strip Size          : 256 KB
Number Of Drives    : 1
Span Depth          : 1
Default Cache Policy: WriteBack, ReadAhead, Direct, No Write Cache if Bad BBU
Current Cache Policy: WriteThrough, ReadAhead, Direct, No Write Cache if Bad BBU
Access Policy       : Read/Write
Disk Cache Policy   : Disk's Default
Encryption Type     : None

Exit Code: 0x00
``` 
Notice we have the `ReadAhead` in `Current Cache Policy`, we need to turn off this parameter in order to let zfs runs fast.     

```
# 关闭缓存
# ./MegaCli64 -LDSetProp -NORA -Immediate -Lall -aAll
.....
Current Cache Policy: WriteThrough, ReadAheadNone, Direct, No Write Cache if Bad BBU
.....
```
Create the raidz2 vmpool via following commands:     

```
# zpool create -f -o ashift=12 vmpool raidz2 /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf /dev/sdg /dev/sdh /dev/sdi
# zpool add -f -o ashift=12 vmpool raidz2 /dev/sdj ~ /dev/sdq
# zpool add -f -o ashift=12 vmpool raidz2 /dev/sdr ~ /dev/sdy
```
### zpool info
Get the information of zfs via following:     

```
# zpool list
NAME     SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP  HEALTH  ALTROOT
vmpool  43.5T   132G  43.4T         -     0%     0%  1.00x  ONLINE  -
# zfs list
NAME                     USED  AVAIL  REFER  MOUNTPOINT
vmpool                  93.6G  29.9T   205K  /vmpool
vmpool/base-100-disk-1  8.17G  29.9T  8.17G  -
vmpool/vm-101-disk-1    45.3G  29.9T  45.3G  -
vmpool/vm-102-disk-1    40.1G  29.9T  40.1G  -
```
Adding the zfs pool into the proxmox on GUI, ignore the steps because it's too simple. 


~~~

#### 1.1.3 步骤3：创建虚拟机

### 1.2 已经做过raid的服务器配置zfs

#### 1.2.1 预处理 - 查看服务器当前磁盘信息

![image-20200121174423392](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200121174423392.png)

#### 1.2.2 ssh登录 - 查看VD信息

```shell
./MegaCli64 -LDInfo -LALL -aAll  # VD0（600G）为根分区，一定不要动！！！
```

![image-20200121174708736](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200121174708736.png)

#### 1.2.3 查看adapter信息 - adapter number

​		**接下来的所有操作都要基于这里查到的adapter  number**

```shell
./MegaCli64 -PDList -aAll | grep -i adapter
```

![image-20200121175313116](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200121175313116.png)

####   1.2.4 删除旧的raid产生的VD

```shell
./MegaCli64 -cfglddel -L1 -a0    # L1 （1 - 1.2.2步骤查到的VD 的Target Id）
./MegaCli64 -cfglddel -L2 -a0    # a0 （0 - 1.2.3步骤查到的adapter的number）
./MegaCli64 -cfglddel -L3 -
a0
```

​	**查看当前VD信息：**

![image-20200213104347960](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200213104347960.png)

#### 1.2.5 查看PD对应磁盘

```shell
# ./MegaCli64 -PDList -aAll | more     两个600G的是0和1, 其他的随便动
```

#### 1.2.6 查看当前服务器有多少块磁盘

```shell
# ./MegaCli64 -PDList -aAll | grep 'Slot Number'
```

![image-20200213110417702](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200213110417702.png)

**这⾥注意, 2,3 是没有，从 4~27 为slot number.** 

#### 1.2.7 查询Enclosure ID

```shell
# ./MegaCli64 -PDList -aAll | grep 'Enclosure' 为9
```

#### 1.2.8 开始做24个raid0: 

```shell
# ./MegaCli64 -CfgLdAdd -r0 [9:4] -a0 
# ./MegaCli64 -CfgLdAdd -r0 [9:5] -a0 
...... 
# ./MegaCli64 -CfgLdAdd -r0 [9:26] -a0 
# ./MegaCli64 -CfgLdAdd -r0 [9:27] -a0
```

#### 1.2.9 lsblk 查看磁盘信息:

![image-20200213111401895](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200213111401895.png)



**删除多余分区sdb/sdn/sdr，删除后磁盘信息如下：**

![image-20200213112555486](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200213112555486.png)

#### 1.2.10 创建zfs pool

```shell
# zpool create -f -o ashift=12 vmpool raidz2 /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf /dev/sdg /dev/sdh /dev/sdi 
# zpool add -f -o ashift=12 vmpool raidz2 /dev/sdj /dev/sdk /dev/sdl /dev/sdm /dev/sdn /dev/sdo /dev/sdp /dev/sdq 
# zpool add -f -o ashift=12 vmpool raidz2 /dev/sdr /dev/sds /dev/sdt /dev/sdu /dev/sdv /dev/sdw /dev/sdx /dev/sdy 
# zpool list 
NAME   SIZE  ALLOC FREE EXPANDSZ FRAG CAP DEDUP HEALTH ALTROOT 
vmpool 130T  1.97M 130T 	- 	  0%   0% 1.00x ONLINE    - 
# zfs list 
NAME   USED AVAIL REFER MOUNTPOINT 
vmpool 819K 89.9T 205K  /vmpool
```

#### 1.2.11 添加zfs pool

![image-20200213113115781](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200213113115781.png)

**使用zfs pool:**

![image-20200213113142222](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200213113142222.png)



```shell
# 查看索引
curl http://192.192.185.63：9200/_cat/indices?pretty | sort
# 删除索引
curl -XDELETE http://localhost:9200/ds-log-2019-11*

```

