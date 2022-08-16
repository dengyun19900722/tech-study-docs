# 简介

​	本目录存放产品生命周期各环节产出的文档，持续积累，形成产品知识库，完成知识共享。

# type

文档类型按大类分为：

- 需求（requirement - R）
- 规划（plan - P）
- 设计（design - DS）
- 开发（develop - DV）
- 发布（release - RL）
- 实施（ops - OPS）
- 项目管理（manage - M）

# 命名规范

文档命名参照以下规范：

```
type-no(随机)-body.md
```

# 知识库说明

## 需求

## 规划

## 设计

## 开发

## 发布

## 实施

## 项目管理

# 如何高效工作？（每周必须花半天来思考）

**1、如何做事**：怎么梳理好需求、跟进好计划，完成从需求到计划、落地的全生命周期跟进？（需求池+计划表！！！ --- xmind配合跟进）
**2、如何带人**：如何较好的发挥组员能动性，只有提高了他们的能动性，能力才可进展，我的压力才能释放！！！

# 打造完美写作系统：Gitbook+Github Pages+Github Actions

https://sinoui.github.io/sinoui-guide/docs/github-pages-introduction



https://sinoui.github.io/sinoui-guide/docs/github-pages-introduction

github pages样例

 利用npm安装gitbook
安装好NodeJS后，可利用它的包管理器（npm）安装gitbook：

npm install gitbook-cli -g
1.


npm的包安装分为本地安装（local）、全局安装（global）两种
本地安装:
npm install xxx 安装到命令行所在目录的node_module目录。
全局安装:
npm install xxx -g 安装到 \AppData\Roaming\npm\node_modules目录



gitbook-cli是一个实用程序，可在同一系统上安装和使用多个版本的gitbook。 它将自动安装所需版本的gitbook。安装好后可查看版本号：



创建
gitbook能创建模板书：

gitbook init [./directory]
1.
最后一个参数为创建的目录，可省略，默认为当前目录下。



命令执行完后会生成两个文件：README.md和SUMMARY.md

README.md文件用来介绍该书籍，SUMMARY.md文件为该书籍的目录。

对SUMMARY.md文件进行如下编辑：

# Summary

* [Introduction](README.md)
* [Easy](easy/README.md)
    * [1.Two Sum](easy/1.Two Sum.md)
    * [7.Reverse Integer](easy/7.Reverse Integer.md)
* [Medium](medium/README.md)
    * [2.Add Two Numbers](medium/2.Add Two Numbers.md)
    * [3.Longest Substring Without Repeating Characters](medium/3.Longest Substring Without Repeating Characters.md)
1.
2.
3.
4.
5.
6.
7.
8.
9.
再次执行命令gitbook init，如果目录里的文件不存在，将会自动创建：



然后我们对其中的md文件编写相应的内容即可。
-----------------------------------
打造完美写作系统：Gitbook+Github Pages+Github Actions
https://blog.51cto.com/phyger/5276526