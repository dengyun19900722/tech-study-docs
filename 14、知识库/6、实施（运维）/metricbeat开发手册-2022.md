# 环境部署

## 打包环境部署

### 安装go

​	完成go环境安装及设置GO_ROOT/GO_PATH环境变量；	

### 安装GVM（非必须，高版本才要）

```shell
curl -sL -o ~/bin/gvm https://github.com/andrewkroh/gvm/releases/download/v0.4.1/gvm-linux-amd64
chmod +x ~/bin/gvm
eval "$(gvm 1.17.8)"
go version
```

### 下载源码

```shell
mkdir -p ${GOPATH}/src/github.com/elastic
git clone https://github.com/elastic/beats ${GOPATH}/src/github.com/elastic/beats
 
# 从tag创建新的分支
cd  ${GOPATH}/src/github.com/elastic/beats
git branch 6.8.3 v6.8.3

#切换到新的分支
git checkout 6.8.3
```

### 设置go module

```shell
# 关闭go module模式，使用go path模式
go env -w GO111MODULE=off

# 按需开启go module模式
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

### 下载mage依赖（非必须，高版本才要）

```shell
go get -u github.com/elastic/beats/v7/dev-tools/mage/target/build
```

### 编译metricbeat

```shell
cd metricbeat/
make

# 试运行，查看日志
sudo ./metricbeat -e -c metricbeat.yml -d "publish" 
```

### FAQ

#### 1、metricbeat 执行make 失败

##### 1.1 问题描述

I make the filebeat on a linux **aarch** machine.

My Environment:

```shell
$ go version
go version go1.11.2 linux/arm64

$ git rev-parse HEAD
79a6ff31aec8e371631661add2e52938f250ceba
```

I get `make` failing with the following error:

```shell
$ make
go build -i -ldflags "-X github.com/elastic/beats/libbeat/version.buildTime=2018-12-06T08:20:44Z -X github.com/elastic/beats/libbeat/version.commit=79a6ff31aec8e371631661add2e52938f250ceba"
# github.com/elastic/beats/libbeat/common/file
../libbeat/common/file/stderr_other.go:30:9: undefined: syscall.Dup2
make: *** [filebeat] Error 2
```

##### 1.2 解决方案

​	问题原因是6.8.3版本在arm上的stdlib系统调用包中缺少Dup2，需要修改代码，重新编译：

```shell
diff --git a/libbeat/common/file/stderr_other.go b/libbeat/common/file/stderr_other.go
index 99ad24c..2731322 100644
--- a/libbeat/common/file/stderr_other.go
+++ b/libbeat/common/file/stderr_other.go
@@ -21,11 +21,11 @@ package file

 import (
        "os"
-       "syscall"
+       "golang.org/x/sys/unix"
 )

 // RedirectStandardError causes all standard error output to be directed to the
 // given file.
 func RedirectStandardError(toFile *os.File) error {
-       return syscall.Dup2(int(toFile.Fd()), 2)
+       return unix.Dup2(int(toFile.Fd()), 2)
 }
```



## 开发环境准备

