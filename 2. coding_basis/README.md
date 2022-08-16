# 编码基础

​	本章节主要记录编码相关的技术点:





```shell
# 安装govendor
go get -u github.com/kardianos/govendor
export PATH="$GOPATH/bin:$PATH"
# 在项目根目录下执行以下命令进行 vendor 初始化：
cd ~/go/src/github.com/containous/traefik
govendor init
govendor fetch +out

```



