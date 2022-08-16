### 1. 整体架构

### 2.

#### 2.1 安装

##### 2.1.1 docker

##### 2.1.2 k8s

##### 2.1.3 helm

https://hub.helm.sh/charts/stable/sonatype-nexus

https://github.com/criteo-cookbooks/nexus3#nexus3_api	

### 3. 迁移

#### 3.1 nexus3.X版本迁移

https://www.jianshu.com/p/7b8d2b0a7d06

##### 3.1.1 步骤1：源环境数据备份

```shell
# 查询nexus实例数据挂载方式
kubectl get po -n admin nexus-xxx -oyaml | grep volume
# 找到挂载目录，备份数据目录（完整nexus-data数据）
tar -czvf nexus-data-backup.tar
```

##### 3.1.2 步骤2：目标环境文件上传、解压

```shell
ssh test@nfs-server-ip:/home/test
# 上传备份数据压缩包至/home/test目录
tar -zxvf nexus-data-backup.tar
cp -rf nexus-data/* /var/nfs/admin-nexus-claim-xxxxxxx
```

##### 3.1.3 步骤3：修复文件目录权限变化

```shell
# 进入nexus的nfs挂载目录
cd /var/nfs/admin-nexus-claim-xxxxxxx
# 删除缓存
rm -rf cache
# 执行权限修改命令
chown -R 200 *
chgrp -R 200 *
```

##### 3.1.4 步骤4：重启nexus实例

```shell
kubectl get rs -n admin | grep nexus | awk '{print $1}' | xargs kubectl -n admin delete rs
```