---
title: 【学习笔记】之git
tags: [学习笔记, git]
categories: [学习笔记]
---
### 克隆指定分支
    git clone -b 分支名 仓库地址
### 不带参数：列出本地已经存在的分支，并且在当前分支的前面用"*"标记
    git branch
### 查看远程版本库分支列表
    git branch -r
### 删除dev分支，如果在分支中有一些未merge的提交，那么会删除分支失败，此时可以使用 git branch
    git branch -d dev
### 给分支重命名
    git branch -m oldName newName
### 将分支切换到master
    git checkout master
### 如果分支存在则只切换分支，若不存在则创建并切换到master分支
    git checkout -b master
### 将本地develop分支合并到本地当前你所在的分支上
    git merge develop
### 推送到远程feature/Interface分支
    git push -u origin feature/Interface
  > '-u' 表示推送全部
### 合并两个独立启动仓库的历史
    git pull origin master --allow-unrelated-histories
### 关联一个远程仓库
    git remote add origin 远程仓库地址
### 免密提交
  	在git文件夹的config配置文件修改url
  	url = https://gitee.com/chen2114/studyNotes.git
  	改成:
  	url = https://账号:密码@gitee.com/chen2114/studyNotes.git
  	url = https://13655079137:cwb073344520@gitee.com/chen2114/studyNotes.git
