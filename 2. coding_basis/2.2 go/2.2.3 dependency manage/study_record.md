### 1. go module

#### 1.1 何为go module？

#### 1.2 搭建本地goproxy

```shell
# goproxy 代理地址配置为本地目录即可
go env -w GOPROXY=file:///$GOPATH/PKG/mod
```

![](C:\Users\dengy\AppData\Roaming\Typora\typora-user-images\image-20200323213823983.png)

```
export GO111MODULE="on"
```

