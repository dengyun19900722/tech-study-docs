# 版本管理规范

![绘图1](DV1-git使用规范.assets/绘图1.png)



# 历史版本需求开发/问题修复

## Step1：将tag处代码check到一个新的分支上

```shell
git checkout -b branch_name tag_name
```

## Step2：在新的分支上开发需求/修复bug，并提交

​		**注：<需求开发/修复bug时>历史版本与主干最新版本逻辑差异较大：**需要在历史版本及主干完成2次开发，并且分别提测；

## Step3：修改完以后，在当前分支打一个tag，并且提交远程

```shell
git tag <tagName> //创建本地tag

git push origin <tagName> //推送到远程仓库

若存在很多未推送的本地标签，你想一次全部推送的话：

git push origin --tags
```

## Step4：合并主分支代码到新建开发分支，在gitlab上提交代码合并请求（merge request）

```shell
git checkout  main     //回到之前的分支

git merge     new_branch    //合并新分支  -- 最好使用idea
```

## Step5：切换到Step3中新打的tag处查看代码是否正确

## Step6：删除check出的那个分支

```shell
git branch -d   new_branch   //删除本地check出来的新分支
git push  origin  --delete   new_branch  //删除远程的新分支
```

# Git分支merge和rebase的区别

https://www.cnblogs.com/zhangsanfeng/p/9575184.html?share_token=59bfb92f-9fa2-45e0-831a-bef5d592c00f