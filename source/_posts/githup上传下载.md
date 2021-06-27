---
title: githup上传下载
date: 2021-06-30 13:48:15
tags: githup
category: githup
---
## 上传
1 首先在githup上新建一个仓库
一定是英文常见，Description (optional)自述信息要写和项目相关的，要不然找不到了
点击创建即可

2：git init 初始化 git
会在文件内有一个.git文件夹

3：git add .
提交文件 .代表这个文件夹内的全部提交上去

4：git commit -m "提交备注"
翻译是首先提交，提交之后 没理解啥意思

5：git branch -M main
获取git分支 - 执行过一次之后就不需要了，

6：git remote add origin https://github.com/A-lyc/XXXXXX.git
获取谁的分支 - 执行过之后，远程仓库已经存在了

6-1：git config --global http.sslVerify "false"
报错，OpenSSL SSL_read: Connection 被重置

7：git push -u origin main
提交

本地新建文件夹，之后执行git init ，之后把项目拖入文件夹内，执行git add . 之后 git commit -m "提交备注"之后git branch -M main之后git remote add origin https://github.com/A-lyc/XXXXXX.git之后git config --global http.sslVerify "false"之后git push -u origin main


## 下载
压缩包下载
下载
1：git clone https://github.com/A-lyc/XXXXXX.git

