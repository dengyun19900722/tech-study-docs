### 1 大纲

- ##### 1.1 命令大全

- ##### 1.2 vim

- 

### 1 命令大全

#### 1.1 优秀博客

| 博客地址                                                     | 博客说明              | 备注 |
| ------------------------------------------------------------ | --------------------- | ---- |
| https://man.linuxde.net/bash                                 | linux命令大全         |      |
| https://wangchujiang.com/linux-command/list.html#!kw=vim     | linux命令快速检索     |      |
| http://cn.linux.vbird.org/linux_basic/0310vi.php             | 鸟哥私房菜 - 工具网站 |      |
| https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md | 命令行的艺术          |      |
| https://explainshell.com/                                    | shell 语法分析器      |      |

#### 1.2 文本编辑器 - vim

##### 1.2.1 快捷键记录

| 操作类别 | 快捷键          | 功能描述                         | 备注 |
| -------- | --------------- | -------------------------------- | ---- |
|          | :q              | quit（退出）                     |      |
|          | :w              | write（写入）                    |      |
|          | :x              | 保存并退出                       |      |
| 删除类   | x               | 删除当前光标下的字符             |      |
|          |                 |                                  |      |
|          | 7dd             | number dd（删除number行内容）    |      |
|          | p               | patste（粘贴）                   |      |
|          | dd + p          | 移动行                           |      |
|          | u               | 撤销操作                         |      |
|          | y3 + 右 + p     |                                  |      |
|          | o               | 下方插入行并进入编辑模式         |      |
|          | shift+o         | 上方插入行并进入编辑模式         |      |
|          |                 |                                  |      |
|          | f + （char）    | find char                        |      |
|          |                 |                                  |      |
|          | number + option | 多次操作（eg：7dd - 删除7行，5） |      |

#### 1.3 文件管理

##### 1.3.1 文件多线程压缩工具 - pigz

> pigz是支持并行的gzip,默认用当前逻辑cpu个数来并发压缩，无法检测个数的话，则并发8个线程：

```shell
# 安装pigz
sudo apt install pigz
# 打包
tar -cvf - hg19_index/ | pigz -p 8 > index_p8.tar.gz
# 解包
pigz -p 8 -d index_p8.tar.gz 
time command -- time：打印command执行时间
```

> **pigz速度测试结果：**

```shell
### 使用gzip进行压缩（单线程）
[20:30 root@hulab /DataBase/Human/hg19]$ time tar -czvf index.tar.gz hg19_index/
hg19_index/
hg19_index/hg19.tar.gz
hg19_index/hg19/
...

real    5m28.824s
user    5m3.866s
sys 0m35.314s

### 使用4线程的pigz进行压缩
[20:38 root@hulab /DataBase/Human/hg19]$ time tar -cvf - hg19_index/ | pigz -p 4 > index_p4.tat.gz 
hg19_index/
hg19_index/hg19.tar.gz
hg19_index/hg19/
...

real    1m18.236s
user    5m22.578s
sys 0m35.933s

### 使用8线程的pigz进行压缩
[20:42 root@hulab /DataBase/Human/hg19]$ time tar -cvf - hg19_index/ | pigz -p 8 > index_p8.tar.gz 
hg19_index/
hg19_index/hg19.tar.gz
hg19_index/hg19/
...

real    0m42.670s
user    5m48.527s
sys 0m28.240s

### 使用16线程的pigz进行压缩
[20:43 root@hulab /DataBase/Human/hg19]$ time tar -cvf - hg19_index/ | pigz -p 16 > index_p16.tar.gz 
hg19_index/
hg19_index/hg19.tar.gz
hg19_index/hg19/
...

real    0m23.643s
user    6m24.054s
sys 0m24.923s
### 使用32线程的pigz进行压缩
[20:43 root@hulab /DataBase/Human/hg19]$ time tar -cvf - hg19_index/ | pigz -p 32 > index_p32.tar.gz 
hg19_index/
hg19_index/hg19.tar.gz
hg19_index/hg19/
...

real    0m17.523s
user    7m27.479s
sys 0m29.283s

### 解压文件
[21:00 root@hulab /DataBase/Human/hg19]$ time pigz -p 8 -d index_p8.tar.gz 

real    0m27.717s
user    0m30.070s
sys 0m22.515s
```

| 程序 | 命令                 | 线程数 | 时间      |
| ---- | -------------------- | ------ | --------- |
| gzip | tar -c z（=gzip） vf | 1      | 5m28.824s |
| pigz |                      | 4      | 1m18.236s |
| pigz |                      | 8      | 0m42.670s |
| pigz |                      | 16     | 0m23.643s |
| pigz |                      | 32     | 0m17.523s |



##### 1.3.2 文件免密交互连接 - sshpass

```shell
sshpass -p "thinker" scp xxxxx test@12.12.1.2:/home/test
```

