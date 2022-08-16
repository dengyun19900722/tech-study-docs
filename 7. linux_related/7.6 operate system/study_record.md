```shell


#（1）在省厅环境保存sf-front镜像：
docker pull portus.teligen.com:5000/admin/sf-front:2.0.0
docker save -o sf-front-image-0221.tar portus.teligen.com:5000/admin/sf-front:2.0.0 
xz sf-front-image-0221.tar
scp sf-front-image-0221.tar.xz root@12.7.0.50:/root  #上传至50服务器
#（2）将sf-front-image-0221.tar.xz 包拷贝至公安网、解压
docker load -i sf-front-image-0221.tar.xz                 #导入镜像
docker push portus.teligen.com:5000/admin/sf-front:2.0.0  #推送至镜像仓库

```

### 0. 优秀博客

#### 1. operating system installation 

##### 1.1 boot option（启动选项）

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

