### 1. 开发环境搭建

#### 1.1 windows环境

##### 1.1.1 build traefik binary

```shell
#设置环境变量
export GOPATH=~/go
export PATH=$PATH:$GOPATH/bin

cd ~/go/src/github.com/containous/traefik

# Get go-bindata. (Important: the ellipses are required.)
GO111MODULE=off go get github.com/containous/go-bindata/...

# Generate UI static files
rm -rf static/ autogen/; make generate-webui

# required to merge non-code components into the final binary,
# such as the web dashboard/UI
go generate

# Standard go build
go build ./cmd/traefik

#解决windows traefik相关依赖缺失问题
go mod download github.com/Microsoft/go-winio@v0.4.14
go mod download github.com/konsorten/go-windows-terminal-sequences@v1.0.2

go get github.com/ryanuber/go-glob #添加新的依赖 - 会写入go.mod、go.sum文件
```

##### 1.1.2  构建镜像（携带traefik全量依赖）

```shell
root@test:~# cd $GOPATH
root@test:~# vim Dockerfile
>FROM nginx
>ADD traefik-v2.0-dependency.tar.gz /tmp
```

```shell
#!/bin/bash
tar -cvf - bin/ pkg/ src/ | pigz -p 8 > traefik-v2.0-dependency.tar.gz	
docker build  -t test-traefik .
docker tag test-traefik ycdocker9527/test-traefik
docker login  -u ycdocker9527 -p ca0987hf
docker push ycdocker9527/test-tr、aefik
```