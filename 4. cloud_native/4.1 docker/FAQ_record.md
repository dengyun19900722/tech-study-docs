### 1. google镜像gcr.io、quay.io等拉不到怎么办？

​	可使用中科大镜像和Azure中国镜像进行拉取;

| 源镜像仓库                         | 中间镜像仓库                                               | 备注                  |
| ---------------------------------- | ---------------------------------------------------------- | --------------------- |
| **gcr.io**/xxx/yyy:zzz             | **gcr**.mirrors.ustc.edu.cn/xxx/yyy:zzz                    | **使用中科大镜像**    |
| **quay.io**/xxx/yyy:zzz            | **quay**.mirrors.ustc.edu.cn/xxx/yyy:zzz                   | **使用中科大镜像**    |
| **k8s.gcr.io**/addon-resizer:1.8.3 | **gcr**.**azk8s**.cn/google-containers/addon-resizer:1.8.3 | **使用Azure中国镜像** |

### 2. docker环境安装

​	docker - ce（https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04）

### 3. docker批量下载镜像脚本

```shell
#!/bin/bash
imageNameArray=(docker.io/coredns/coredns:1.3.0 docker.io/library/nginx:alpine docker.io/library/traefik:1.7.12 docker.io/rancher/klipper-helm:v0.1.5 docker.io/rancher/klipper-lb:v0.1.1 k8s.gcr.io/pause:3.1)
for(( i=0;i<${#imageNameArray[@]};i++)) do
sudo ctr images pull ${imageNameArray[i]} --all-platforms
done;
```

### 4. containerd镜像导出失败？

https://github.com/containerd/containerd/issues/3340

### 5. harbor api怎样调用？

https://cloud.tencent.com/developer/article/1151425 

### 6. 修改harbor网段（IP段）

https://github.com/goharbor/harbor/issues/2634





```
sudo apt-get install -y ./XXX.deb
```

