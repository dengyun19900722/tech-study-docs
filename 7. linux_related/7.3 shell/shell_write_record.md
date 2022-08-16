# **1.** 软件包管理

## **1.1.** ***软件包下载（制作本地软件源）***

https://www.cnblogs.com/gzxbkk/p/7809296.html（demo）

### **1.1.1.** ***通过源获取***

### **1.1.2.** ***Docker容器方式获取***

#### **1.1.2.1.** ***步骤1：运行容器***

```shell
sudo docker run -i -t arm64v8/ubuntu:18.04 /bin/bash
```

#### **1.1.2.2.** ***步骤2：更新本地源***

```shell
apt-get update -y

# 若有需要，可以更新源信息 - 变更为阿里云
vim /etc/apt/source.list

deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

```



#### **1.1.2.3.** ***步骤2：修改容器内源管理配置***

```shell
apt-get install vim
cd /etc/apt/apt.conf.d/
vim docker-clean       #清空内容
```

 **1.1.2.4.** ***步骤4：开始安装需要的软件***

```shell
apt-get install vsftpd
```

#### **1.1.2.5.** ***步骤5：生成包的依赖信息***

```shell
cd /var/cache
mkdir -p /root/debs
find . | grep deb$ | xargs -I % cp % /root/debs
cd /root/debs
apt-get install -y dpkg-dev                             #安装dpkg-dev包管理工具
dpkg-scanpackages . /dev/null | gzip -9c > Packages.gz 	# 打包deb报的依赖关联关系
```

 

#### **1.1.2.6.** ***步骤6：拷贝到内网，制作本地软件源***

```shell
vim /etc/apt/source.list                   #编辑软件源配置文件
deb ftp://10.10.10.25/  ubuntu-L-package/   #新增配置源
apt-get update                          #更新软件源信息  == windows检查软件更新
apt-get upgrade                         #升级软件
```

 

## **1.2** ***附录I 源管理相关命令***

| 命令             | 命令说明                             | 备注 |
| ---------------- | ------------------------------------ | ---- |
| cat /etc/issue   | 查看ubuntu系统版本号                 |      |
| apt list         | 列出包含条件的包（已安装，可升级等） |      |
| apt edit-sources | 编辑源列表                           |      |
|                  |                                      |      |
|                  |                                      |      |
|                  |                                      |      |
|                  |                                      |      |

 

```shell
1  cat /etc/issue

  2  apt-get update -y

  3  apt-get install -y vim

  4  vim /etc/apt/apt.conf.d/01autoremove

  5  vim /etc/apt/apt.conf.d/docker-autoremove-suggests 

  6  cd /etc/apt/apt.conf.d/

  7  ls

  8  vim 01autoremove

  9  vim docker-autoremove-suggests 

  10  vim docker-clean 

  11  apt-get install -y nfs-kernel-server

  12  cd /var/cacche

  13  cd /var/cache

  14  find . | grep deb

  15  mkdir -p /root/debs

  16  find . | grep deb$ | xargs -I % cp % /root/debs

  17  ls /root/debs

  18  apt-get install -y dpkg-deveel

  19  apt-get install -y dpkg-devel

  20  apt-get install -y dpkg-dev

  21  cd root/debs

  22  ls

  23  cd /root/debs

  24  ls

  25  dpkg-scanpackages . /dev/null | gzip -9c > Packages.gz
```

# 2 文件操作

## 2.1 文件内容变更