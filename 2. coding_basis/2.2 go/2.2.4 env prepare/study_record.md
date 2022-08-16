### 1. go环境准备

#### 1.1 linux

1、下载安装包

```shell
wget https://storage.googleapis.com/golang/go1.14.1.linux-amd64.tar.gz
```

2、执行安装脚本、完成安装

```shell
#安装、升级脚本
#!/bin/bash
if [ -z "$1" ]; then
        echo "usage: ./install.sh go-package.tar"
        exit
fi

if [ -d "/usr/local/go" ]; then
        echo "Uninstalling old version..."
        sudo rm -rf /usr/local/go
fi
echo "Installing..."
sudo tar -C /usr/local -xzf $1
echo "Done"
```

3、配置环境变量

```shell
$ sudo vi /etc/profile
export GOPATH=/root/go
export GOROOT=/usr/local/go
export PATH=$PATH:$GOPATH/bin
export GO111MODULE=on      #高版本go（>v1.11）打开module包管理模式、取代go get
$ source /etc/profile

cd $GOPATH/src/github.com/containous/traefik
go build ./cmd/traefik/
```

#### 1.2 windows





### 2.  git环境准备

```shell
sudo yum install git
```

http://blueskykong.com/2019/03/01/go-dep-3/

