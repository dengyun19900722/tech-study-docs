# 1 安装

## 1.1 ubuntu

### 1.1.1 **安装依赖包**

```shell
#!/bin/bash
pkgArr=(bzip2 elfutils automake autotools-dev binutils expat gawk gcc gcc-multilib g++-multilib ia32-libs ksh less lesstif2 lesstif2-dev lib32z1 libaio1 libaio-dev libc6-dev libc6-dev-i386 libc6-i386 libelf-dev libltdl-dev libmotif4 libodbcinstq4-1 libodbcinstq4-1:i386 libpth-dev libpthread-stubs0 libpthread-stubs0-dev libstdc++5 lsb-cxx make openssh-server pdksh rlwrap rpm sysstat unixodbc unixodbc-dev unzip x11-utils zlibc)

for pkg in ${pkgArr[@]}
#也可以写成for element in ${array[*]}
do
  echo "================================================================================"
  echo "start to install "$pkg
  apt-get install -y $pkg
  echo "end to install "$pkg
  echo "================================================================================"
done
```

## 1.2 centos

​		很多时候，oracle是安装在服务器上的Linux环境下。因为服务器的特殊性，一般安装的Linux环境都是不带有图形界面的，这时就要求oracle在命令行模式下进行安装。本文档综合网上的各类oracle静默安装的实例，结合自己的理解，经过实际安装操作，得出了一种可行的静默安装的流程，仅供参考。该文章仅针对于centos7 oracle 11g R2版本的静默安装，即非图形界面的安装，安装流程如下：

### **1.2.1** ***第一步：检查软件安装包***

​		通过 rpm -qa |grep 包名的方式检查是否安装了对应的包，如果安装，会显示对应的包的版本号的，如果没有安装，则不会显示任何内容

```shell
$ rpm -qa |grep binutils
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps1.png) 

​	如果没有安装，则通过yum install 包名 的方式进行安装对应的软件包。

```shell
$ yum install binutils
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps2.png) 

​		依次检查如下软件包

```shell
binutils
compat-libcap1
compat-libstdc++-33
elfutils-libelf
elfutils-libelf-devel
gcc
gcc-c++
glibc
ksh
libgcc
libstdc++
libaio
libaio-devel
make
sysstat
unixODBC
unixODBC-devel
```

以上环境是安装oracle必须具备的，若缺少某项环境，可能在安装过程中出现某些未知的错误。

```shell
# 操作脚本
#!/bin/bash
pkgArr=(binutils compat-libcap1 compat-libstdc++-33 elfutils-libelf elfutils-libelf-devel gcc gcc-c++ glibc ksh libgcc libstdc++ libaio libaio-devel make sysstat unixODBC unixODBC-devel)
failedPkg=''
for pkg in ${pkgArr[@]}
#也可以写成for element in ${array[*]}
do
  echo "================================================================================"
  echo "start to install "$pkg
  apt-get install -y $pkg
  #验证包是否安装成功
  isSuccess =$(rpm -qa | grep $pkg | wc -l)
  if[$isSuccess = "0"];then
    $failedPkg=$failedPkg","$pkg
  fi
  echo "end to install "$pkg
  echo "================================================================================"
done
```



### **1.2.2 **第二步：修改kernel参数

```shell
$ vim /etc/sysctl.conf

#在底部添加如下内容：
net.ipv4.ip_local_port_range = 9000 65500
fs.file-max = 6815744
kernel.shmall = 10523004
kernel.shmmax = 6465333657
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.core.rmem_default=262144
net.core.wmem_default=262144
net.core.rmem_max=4194304
net.core.wmem_max=1048576
fs.aio-max-nr = 1048576
```

 

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps3.png) 

​		让内核参数生效:

```shell
$ sysctl -p
```

```shell
# 操作脚本
#!/bin/bash
cat << EOF >> /etc/sysctl.conf
net.ipv4.ip_local_port_range = 9000 65500
fs.file-max = 6815744
kernel.shmall = 10523004
kernel.shmmax = 6465333657
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.core.rmem_default=262144
net.core.wmem_default=262144
net.core.rmem_max=4194304
net.core.wmem_max=1048576
fs.aio-max-nr = 1048576
EOF
# 使内核参数生效
sysctl -p
```



### **1.2.3** ***第三步：创建用户和相应的组***

```shell
$ groupadd -g 200 oinstall
$ groupadd -g 201 dba
$ useradd -u 440 -g oinstall -G dba -d /home/oracle oracle
# change password of user - oracle
$ passwd oracle
```

```
groupadd -g 200 oinstall
$ groupadd -g 201 dba
$ useradd -u 440 -g oinstall -G dba -d /home/oracle oracle
```



### **1.2.4** ***第四步：修改登录系统参数***

```shell
$vim /etc/pam.d/login
在底部添加:
session required pam_limits.so

```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps4.png) 

### **1.2.5** ***第五步：修改操作系统安全限制等参数***

```shell
$ vim /etc/security/limits.conf
#在文件底部添加
oracle soft nproc 2047
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps5.png) 

### **1.2.6** ***第六步：修改环境变量文件***

```shell
$ vim /etc/profile
#在文件末尾添加
if [ $USER = "oracle" ]; then
	if [ $SHELL = "/bin/ksh" ]; then
		ulimit -p 16384
		ulimit -n 65536
	else
		ulimit -u 16384 -n 65536
	fi
fi
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps6.png) 

​		使环境变量生效

```shell
$ source profile
```



### 1.2.7 ***第七步：创建用户目录并分配权限***

```shell
$ mkdir /oracle
#对oracle目录进行授权
$ chmod 777 /oracle
$ chown -R oracle:oinstall /oracle
```

 

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps7.png) 

### **1.2.8** **第八步：上传、解压oracle安装包**

```shell
#$上传安装包linux.x64_11gR2_database_1of2.zip、linux.x64_11gR2_database_2of2.zip至/tmp/目录
$ cd /tmp/
$ unzip linux.x64_11gR2_database_1of2.zip
$ unzip linux.x64_11gR2_database_2of2.zip
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps10.png) 

解压后。同一级目录中会生成一个database文件夹。

### **1.2.9 ** ***第九步：拷贝database/response/下面的dbca.rsp、db_install.rsp及netca.rsp三个文件至/home/oracle目录中***

```shell
$ cp database/response/dbca.rsp /home/oracle/
$ cp database/response/db_install.rsp /home/oracle/
$ cp database/response/netca.rsp /home/oracle/
```

 ![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps11.png) 

### **1.2.10 ** ***第十步：编辑db_install.rsp文件***

```shell
$ cd /home/oracle
$ vim db_install.rsp
#修改如下参数:
Oracle.install.option=INSTALL_DB_SWONLY
ORACLE_HOSTNAME= master
UNIX_GROUP_NAME=oinstall
INVENTORY_LOCATION=/oracle/oraInventory
SELECTED_LANGUAGES=en,zh_CN
ORACLE_HOME=/oracle/product/11.2.0/dbhome_1
ORACLE_BASE=/oracle
Oracle.install.db.InstallEdition=EE
Oracle.install.db.DBA_GROUP=dba
Oracle.install.db.OPER_GROUP=oinstall
Oracle.install.db.config.starterdb.type=GENERAL_PURPOSE
Oracle.install.db.config.starterdb.globalDBName=orcl
Oracle.install.db.config.starterdb.SID=orcl
Oracle.install.db.config.starterdb.characterSet=ZHS16GBK
Oracle.install.db.config.starterdb.password.ALL=oracle
DECLINE_SECURITY_UPDATES=true
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps12.png) 

### **1.2.12 ** ***第十一步：切换至oracle 帐号，添加oracle用户下面的环境变量、创建oraInventory目录。***

```shell
$ su - oracle
# 新建oraInventory文件夹
$ mkdir /oracle/oraInventory
$ vim ~/.bash_profile
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps8.png) 

​	在.base_profile里面添加如下内容

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps9.png) 

```shell
# 补充实例id环境变量
export ORACLE_SID=ora11g
# 使配置生效
source ~/.bash_profile
```

**ORACLE_HOME在配置时注意后面不要带有:分隔符，否则PATH会取不到ORACLE_HOME的路径**

### **1.2.12 ** ***第十二步：开始执行安装操作***

```shell
$ ./runInstaller -silent -ignorePrereq -responseFile /home/oracle/db_install.rsp
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps13.png) 

然后进/oracle/oraInventory/logs/查看对应的安装日志文件

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps14.png)![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps15.png)		遇到上述提示时，执行以下操作：

​		**切换至root帐号，执行两个脚本命令**

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps16.png) 

### **1.2.13 ** ***第十三步：新开一个客户端，root帐号下面执行orainstRoot.sh及root.sh两脚本***

```shell
$ cd /oracle/oraInventory
$ sh orainstRoot.sh
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps17.png)

```shell
$ cd /oracle/product/11.2.0/dbhome_1
$ sh root.sh
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps18.png) 

### **1.2.14 ** ***第十四步：安装监听,直接执行监听脚本***

```shell
# oracle用户下执行
$ netca /silent /responseFile /home/oracle/netca.rsp
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps19.png) 

### **1.2.15 ** ***第十五步：安装实例前准备 - 修改***responseFile

```shell
$vim /home/oracle/dbca.rsp
#修改以下配置
GDBNAME=tdr
SID=tdr
DATAFILEDESTINATION=/oracle/oradata
RECOVERYAREADESTINATION=/oracle/flash_recovery_area
CHARACTERSET=ZHS16GBK
TOTALMEMORY=800
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps20.png) 

### **1.2.16 **第十六步：静默模式创建实例

```shell
$ dbca -silent -responseFile /home/oracle/dbca.rsp
```

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps21.jpg) 

输入sys的密码：sys，enter进入下面的安装界面：

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps22.png) 

### **1.2.17 ** ***第十七步：测试oracle***

```shell
$ sqlplus / as sysdba
```

#### **1.2.17.1** ***问题1：没法登录sysdba***

![img](file:///C:\Users\dengy\AppData\Local\Temp\ksohtml7308\wps23.jpg) 

**（1）故障原因**
	诡异的故障背后的原因竟然是那样的基础：ORACLE_SID没有指定！
	确认系统当前的ORACLE_HOME和ORACLE_SID环境变量

```shell
[oracle@asdlabdb01 ~]$ echo $ORACLE_HOME
/oracle/app/oracle/product/10.2.0/db_1
[oracle@asdlabdb01 ~]$ echo $ORACLE_SID

[oracle@asdlabdb01 ~]$
```

可见，此时只设置了ORACLE_HOME环境变量，但ORACLE_SID此时为空，这就是该问题的真实原因。

（2）***故障处理***
	设置ORACLE_SID，重新尝试登录。

```shell
[oracle@asdlabdb01 ~]$ export ORACLE_SID=ora11g
[oracle@asdlabdb01 ~]$ echo $ORACLE_SID
```

#### **1.2.18.2 ** ***服务器重启后，重新启动oracle监听 + 数据库服务步骤***

```shell
（1）su - oracle          切用户
（2）查看监听状态
    lsnrctl status     
（2）启动监听（监听未启动时）：
    lsnrctl start
（https://blog.csdn.net/zhou920786312/article/details/77413891 （监听无法启动问题解决方案））
（4）进入sqlplus：sqlplus / as sysdba     
（5）启动数据库服务：
     startup             
     常用命令：shutdown   停服务
             exit    退出 
（6）查看监听器日志：
     vim /oracle/diag/tnslsnr/localhost/listener/alert/log.xml
```

 

 

### 1.2.19 完整安装脚本

说明：执行脚本前先上传安装包、解压；脚本执行完毕后，手动创建实例 - 见：1.2.16

```shell
# 操作脚本
#!/bin/bash
exit_script(){
    exit 1
}

echo "step 1: 安装oracle相关依赖包 -- start"
pkgArr=(binutils compat-libcap1 compat-libstdc++-33 elfutils-libelf elfutils-libelf-devel gcc gcc-c++ glibc ksh libgcc libstdc++ libaio libaio-devel make sysstat unixODBC unixODBC-devel)
for pkg in ${pkgArr[@]}
#也可以写成for element in ${array[*]}
do
  echo "================================================================================"
  echo "start to install "$pkg
  apt-get install -y $pkg
  #验证包是否安装成功
  isSuccess =$(rpm -qa | grep $pkg | wc -l)
  if[$isSuccess = "0"];then
    echo "依赖包安装失败，请检查："$pkg
    exit_script
  fi
  echo "安装失败的包如下："$failedPkg
  echo "end to install "$pkg
  echo "================================================================================"
done
echo "step 1: 安装oracle相关依赖包 -- end"

echo "step 2: 修改kernel参数 -- start"
cat << EOF >> /etc/sysctl.conf
net.ipv4.ip_local_port_range = 9000 65500
fs.file-max = 6815744
kernel.shmall = 10523004
kernel.shmmax = 6465333657
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.core.rmem_default=262144
net.core.wmem_default=262144
net.core.rmem_max=4194304
net.core.wmem_max=1048576
fs.aio-max-nr = 1048576
EOF
# 使内核参数生效
sysctl -p
echo "step 2: 修改kernel参数 -- end"

echo "step 3: 创建用户和相应的组 -- start"
groupadd -g 200 oinstall
groupadd -g 201 dba
useradd -u 440 -g oinstall -G dba -d /home/oracle oracle
# change password of user - oracle
echo "123456aB" | passwd "oracle" --stdin
echo "step 3: 创建用户和相应的组 -- end"

echo "step 4: 修改kernel参数 -- start"
cat << EOF >> /etc/pam.d/login
session required pam_limits.so
EOF
echo "step 4: 修改kernel参数 -- end"

echo "step 5: 修改操作系统安全限制等参数 -- start"
cat << EOF >> /etc/security/limits.conf
oracle soft nproc 2047
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
EOF
echo "step 5: 修改操作系统安全限制等参数 -- end"

echo "step 6: 修改环境变量文件 -- start"
cat << EOF >> /etc/profile
if [ $USER = "oracle" ]; then
	if [ $SHELL = "/bin/ksh" ]; then
		ulimit -p 16384
		ulimit -n 65536
	else
		ulimit -u 16384 -n 65536
	fi
fi
EOF
# 使配置生效
source profile
echo "step 6: 修改环境变量文件 -- end"

echo "step 7: 创建用户目录并分配权限 -- start"
mkdir /oracle
chmod 777 /oracle
chown -R oracle:oinstall /oracle
echo "step 7: 创建用户目录并分配权限 -- end"

echo "step 8: 拷贝database/response/下面的dbca.rsp、db_install.rsp及netca.rsp三个文件至/home/oracle目录中 -- start"
cp /tmp/database/response/dbca.rsp /home/oracle/
cp /tmp/database/response/db_install.rsp /home/oracle/
cp /tmp/database/response/netca.rsp /home/oracle/
echo "step 8: 拷贝database/response/下面的dbca.rsp、db_install.rsp及netca.rsp三个文件至/home/oracle目录中 -- end"

echo "step 9: 编辑db_install.rsp文件 -- start"
sed 's/^Oracle.install.option/Oracle.install.option=INSTALL_DB_SWONLY\n/' /home/oracle/db_install.rsp
sed 's/^ORACLE_HOSTNAME/ORACLE_HOSTNAME=master\n/' /home/oracle/db_install.rsp
sed 's/^UNIX_GROUP_NAME/UNIX_GROUP_NAME=oinstall\n/' /home/oracle/db_install.rsp
sed 's/^INVENTORY_LOCATION/INVENTORY_LOCATION=\/oracle\/oraInventory\n/' /home/oracle/db_install.rsp
sed 's/^SELECTED_LANGUAGES/SELECTED_LANGUAGES=en,zh_CN\n/' /home/oracle/db_install.rsp
sed 's/^ORACLE_HOME/ORACLE_HOME=\/oracle\/product\/11.2.0\/dbhome_1\n/' /home/oracle/db_install.rsp
sed 's/^ORACLE_BASE/ORACLE_BASE=\/oracle\n/' /home/oracle/db_install.rsp
sed 's/^Oracle.install.db.InstallEdition/Oracle.install.db.InstallEdition=EE\n/' /home/oracle/db_install.rsp
sed 's/^Oracle.install.db.DBA_GROUP/Oracle.install.db.DBA_GROUP=dba\n/' /home/oracle/db_install.rsp
sed 's/^Oracle.install.db.OPER_GROUP/Oracle.install.db.OPER_GROUP=oinstall\n/' /home/oracle/db_install.rsp
sed 's/^Oracle.install.db.config.starterdb.type/Oracle.install.db.config.starterdb.type=GENERAL_PURPOSE
\n/' /home/oracle/db_install.rsp
sed 's/^Oracle.install.db.config.starterdb.globalDBName/Oracle.install.db.config.starterdb.globalDBName=$1\n/' /home/oracle/db_install.rsp
sed 's/^Oracle.install.db.config.starterdb.SID/Oracle.install.db.config.starterdb.SID=$1\n/' /home/oracle/db_install.rsp

sed 's/^Oracle.install.db.config.starterdb.characterSet/Oracle.install.db.config.starterdb.characterSet=ZHS16GBK\n/' /home/oracle/db_install.rsp

sed 's/^Oracle.install.db.config.starterdb.password.ALL/Oracle.install.db.config.starterdb.password.ALL=oracle
\n/' /home/oracle/db_install.rsp

sed 's/^DECLINE_SECURITY_UPDATES/DECLINE_SECURITY_UPDATES=true\n/' /home/oracle/db_install.rsp
echo "step 9: 编辑db_install.rsp文件 -- end"

echo "step 10: 切换至oracle 帐号，添加oracle用户下面的环境变量、创建oraInventory目录 -- start"
su - oracle
# 新建oraInventory文件夹
mkdir /oracle/oraInventory
vim ~/.bash_profile
cat << EOF >> /etc/profile
PATH=$PATH:$HOME/bin
umask 022
export PATH
export ORACLE_BASE=/oracle
export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/dbhome_1
export PATH=$PATH:$ORACLE_HOME/bin:
export ORACLE_SID=ora11g
EOF

# 使配置生效
source ~/.bash_profile
echo "step 10: 切换至oracle 帐号，添加oracle用户下面的环境变量、创建oraInventory目录 -- end"

echo "step 11: 开始执行安装操作 -- start"
/tmp/database/runInstaller -silent -ignorePrereq -responseFile /home/oracle/db_install.rsp
echo "step 11: 开始执行安装操作 -- end"

echo "step 12: 修改kernel参数 -- start"
# 退出oracle账号，进入root账号
exit 
sh /oracle/oraInventory/orainstRoot.sh
sh /oracle/product/11.2.0/dbhome_1/root.sh
echo "step 12: 修改kernel参数 -- end"

echo "step 13: 安装监听,直接执行监听脚本 -- start"
# oracle用户下执行
su - oracle
$ netca /silent /responseFile /home/oracle/netca.rsp
echo "step 13: 安装监听,直接执行监听脚本 -- end"

echo "step 14: 安装实例前准备 - 修改responseFile -- start"
sed 's/^GDBNAME/GDBNAME=$1\n/' /home/oracle/dbca.rsp
sed 's/^SID/SID=$1\n/' /home/oracle/dbca.rsp
sed 's/^DATAFILEDESTINATION/DATAFILEDESTINATION=\/oracle\/oradata\n/' /home/oracle/dbca.rsp
sed 's/^RECOVERYAREADESTINATION/RECOVERYAREADESTINATION=\/oracle\/flash_recovery_area\n/' /home/oracle/dbca.rsp
sed 's/^CHARACTERSET/CHARACTERSET=ZHS16GBK\n/' /home/oracle/dbca.rsp
sed 's/^TOTALMEMORY/TOTALMEMORY=800\n/' /home/oracle/dbca.rsp
echo "step 14: 安装实例前准备 - 修改responseFile -- end"
```

