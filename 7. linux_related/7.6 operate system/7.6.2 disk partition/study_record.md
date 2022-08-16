# 1 磁盘手动分区

## 1.1 LVM创建和管理

![image-20200331222723697](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200331222723697.png)

### 1.1.1 虚拟机添加两块磁盘，并识别为sdb和sdc**（裸盘）**

[![img](https://www.linuxidc.com/upload/2017_05/170513082351056.png)](https://www.linuxidc.com/upload/2017_05/170513082351056.png)

 

![img](https://www.linuxidc.com/upload/2017_05/170513082351057.png)

 

### 1.1.2 创建PV，使用sdb和sdc

```
[root@localhost ~]``# pvcreate /dev/sdb
 ``Physical volume ``"/dev/sdb"` `successfully created
[root@localhost ~]``# pvcreate /dev/sdc
 ``Physical volume ``"/dev/sdc"` `successfully created
[root@localhost ~]``# pvs
 ``PV     VG  Fmt Attr PSize PFree
 ``/dev/sda2` `vg00 lvm2 a-- 7.80g  0 
 ``/dev/sdb`  `vgzx lvm2 a-- 8.00g 8.00g
 ``/dev/sdc`  `vgzx lvm2 a-- 8.00g 8.00g
```

### 1.1.3 创建VG，使用刚创建的两个PV

```
[root@localhost ~]``# vgcreate vgzx /dev/sdb /dev/sdc
 ``Volume group ``"vgzx"` `successfully created
[root@localhost ~]``# vgs
 ``VG  ``#PV #LV #SN Attr  VSize VFree 
 ``vg00  1  2  0 wz--n- 7.80g   0 
 ``vgzx  2  0  0 wz--n- 15.99g 15.99g
```

###  1.1.4 创建LV，在刚创建的VG上创建两个LV

```
[root@localhost ~]``# lvcreate -L 2G -n lvzx01 vgzx
 ``Logical volume ``"lvzx01"` `created.
[root@localhost ~]``# lvcreate -L 2G -n lvzx02 vgzx
 ``Logical volume ``"lvzx02"` `created.
[root@localhost ~]``# lvs
 ``LV   VG  Attr    LSize Pool Origin Data% Meta% Move Log Cpy%Sync Convert
 ``data  vg00 -wi-ao---- 5.85g                          
 ``swap  vg00 -wi-ao---- 1.95g                          
 ``lvzx01 vgzx -wi-a----- 2.00g                          
 ``lvzx02 vgzx -wi-a----- 2.00g
```

### 1.1.5 使用刚创建的LV创建文件系统并挂载到操作系统

```
[root@localhost ~]# mkfs.ext4 /dev/vgzx/lvzx01 
mke2fs 1.41.12 (17-May-2010)
Filesystem label=
OS type: Linux
Block ``size``=4096 (log=2)
Fragment ``size``=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
131072 inodes, 524288 blocks
26214 blocks (5.00%) reserved ``for` `the super ``user
First` `data block=0
Maximum filesystem blocks=536870912
16 block groups
32768 blocks per ``group``, 32768 fragments per ``group
8192 inodes per ``group
Superblock backups stored ``on` `blocks: 
  ``32768, 98304, 163840, 229376, 294912
 
Writing inode tables: done              
Creating journal (16384 blocks): done
Writing superblocks ``and` `filesystem accounting information: done
 
This filesystem will be automatically checked every 35 mounts ``or
180 days, whichever comes ``first``. Use tune2fs -c ``or` `-i ``to` `override.
[root@localhost ~]# mkfs.ext4 /dev/vgzx/lvzx02 
mke2fs 1.41.12 (17-May-2010)
Filesystem label=
OS type: Linux
Block ``size``=4096 (log=2)
Fragment ``size``=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
131072 inodes, 524288 blocks
26214 blocks (5.00%) reserved ``for` `the super ``user
First` `data block=0
Maximum filesystem blocks=536870912
16 block groups
32768 blocks per ``group``, 32768 fragments per ``group
8192 inodes per ``group
Superblock backups stored ``on` `blocks: 
  ``32768, 98304, 163840, 229376, 294912
 
Writing inode tables: done              
Creating journal (16384 blocks): done
Writing superblocks ``and` `filesystem accounting information: done
 
This filesystem will be automatically checked every 20 mounts ``or
180 days, whichever comes ``first``. Use tune2fs -c ``or` `-i ``to` `override.
[root@localhost ~]# mkdir /orasoft
[root@localhost ~]# mkdir /oradata
[root@localhost ~]# mount /dev/mapper/vgzx-lvzx01 
mount: can't find /dev/mapper/vgzx-lvzx01 ``in` `/etc/fstab ``or` `/etc/mtab
[root@localhost ~]# mount /dev/mapper/vgzx-lvzx01 /oradata/
[root@localhost ~]# mount /dev/mapper/vgzx-lvzx02 /orasoft/
[root@localhost ~]# df -h
Filesystem      ``Size` `Used Avail Use% Mounted ``on
/dev/mapper/vg00-data
           ``5.7G 1.9G 3.6G 34% /
tmpfs         499M   0 499M  0% /dev/shm
/dev/sda1       190M  36M 145M 20% /boot
/dev/mapper/vgzx-lvzx01
           ``2.0G 3.0M 1.9G  1% /oradata
/dev/mapper/vgzx-lvzx02
           ``2.0G 3.0M 1.9G  1% /orasoft
```

 

如果想挂载随机启动需要修改/etc/fastab文件。

### 1.1.6 扩展LV

 

```
[root@localhost ~]# lvextend -L +2G /dev/mapper/vgzx-lvzx01 
 ``Size` `of` `logical volume vgzx/lvzx01 changed ``from` `2.00 GiB (512 extents) ``to` `4.00 GiB (1024 extents).
 ``Logical volume lvzx01 successfully resized
[root@localhost ~]# lvextend -L +3G /dev/mapper/vgzx-lvzx02
 ``Size` `of` `logical volume vgzx/lvzx02 changed ``from` `2.00 GiB (512 extents) ``to` `5.00 GiB (1280 extents).
 ``Logical volume lvzx02 successfully resized
[root@localhost ~]# lvs
 ``LV   VG  Attr    LSize Pool Origin Data% Meta% ``Move` `Log Cpy%Sync ``Convert
 ``data  vg00 -wi-ao``---- 5.85g                          
 ``swap  vg00 -wi-ao``---- 1.95g                          
 ``lvzx01 vgzx -wi-ao``---- 4.00g                          
 ``lvzx02 vgzx -wi-ao``---- 5.00g                          
[root@localhost ~]# df -h
Filesystem      ``Size` `Used Avail Use% Mounted ``on
/dev/mapper/vg00-data
           ``5.7G 1.9G 3.6G 34% /
tmpfs         499M   0 499M  0% /dev/shm
/dev/sda1       190M  36M 145M 20% /boot
/dev/mapper/vgzx-lvzx01
           ``2.0G 3.0M 1.9G  1% /oradata
/dev/mapper/vgzx-lvzx02
           ``2.0G 3.0M 1.9G  1% /orasoft
```

 

### 1.1.7 扩展文件系统resize2fs

​	从上面可以看出，LV分别做了扩展，但在操作系统上还没有显示为扩展

```
[root@localhost ~]# resize2fs /dev/mapper/vgzx-lvzx01 
resize2fs 1.41.12 (17-May-2010)
Filesystem ``at` `/dev/mapper/vgzx-lvzx01 ``is` `mounted ``on` `/oradata; ``on``-line resizing required
old desc_blocks = 1, new_desc_blocks = 1
Performing an ``on``-line resize ``of` `/dev/mapper/vgzx-lvzx01 ``to` `1048576 (4k) blocks.
The filesystem ``on` `/dev/mapper/vgzx-lvzx01 ``is` `now 1048576 blocks long.
 
[root@localhost ~]# resize2fs /dev/mapper/vgzx-lvzx02
resize2fs 1.41.12 (17-May-2010)
Filesystem ``at` `/dev/mapper/vgzx-lvzx02 ``is` `mounted ``on` `/orasoft; ``on``-line resizing required
old desc_blocks = 1, new_desc_blocks = 1
Performing an ``on``-line resize ``of` `/dev/mapper/vgzx-lvzx02 ``to` `1310720 (4k) blocks.
The filesystem ``on` `/dev/mapper/vgzx-lvzx02 ``is` `now 1310720 blocks long.
 
[root@localhost ~]# df -h
Filesystem      ``Size` `Used Avail Use% Mounted ``on
/dev/mapper/vg00-data
           ``5.7G 1.9G 3.6G 34% /
tmpfs         499M   0 499M  0% /dev/shm
/dev/sda1       190M  36M 145M 20% /boot
/dev/mapper/vgzx-lvzx01
           ``3.9G 4.0M 3.7G  1% /oradata
/dev/mapper/vgzx-lvzx02
           ``4.9G 4.0M 4.7G  1% /orasoft
```

# 2. 磁盘分区大小重新调整

## 2.1 centos 7 

### 2.1.1 基础命令

| 命令                          | 命令说明           |      |
| ----------------------------- | ------------------ | ---- |
| df -h                         | 查看磁盘的空间大小 |      |
| pvdisplay   / pvs（简要信息） | 查看物理卷         |      |
| vgdisplay  / vgs              | 查看卷组           |      |
| lvdisplay  /  lvs             | 查看逻辑卷         |      |

### 2.1.2 操作步骤

![image-20200331220009814](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200331220009814.png)

```shell
# step1：备份
/home : cp -r /home/ homebak/
# step2：卸载/home ： 
umount /home
# step3：如果出现 home 存在进程，使用 
fuser -m -v -i -k /home # 终止 home 下的进程，最后使用 umount /home 卸载 /home
# step4：删除/home所在的lv ：
lvremove /dev/mapper/centos-home
# step5：扩展/root所在的lv，增加4430G ： 
lvextend -L +4430G /dev/mapper/centos-root
# step6：扩展/root文件系统 ： 
xfs_growfs /dev/mapper/centos-root
# step7：重新创建home lv ： 
vgdisplay  #查出卷组为centos
lvcreate -L 167G -n home centos # 重新创建home lv分区的大小，根据vgdisplay中的free PE 的大小确定
# step8：创建文件系统：  ==  新建的lv格式化
mkfs.xfs /dev/centos/home
# step9：挂载 home： 
mount /dev/centos/home /home
```

