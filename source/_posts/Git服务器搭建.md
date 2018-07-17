---
title: Git服务器搭建
date: 2018-07-17 09:56:39
tags: [git,github]
toc: true
---
在公司内网搭建git服务器...
<!--more-->
公司源码比较重要，所以为了版本控制和代码安全需要在公司服务器上搭建git

### 准备工作

1. 下载[git](https://github.com/git/git/releases)
2. 解压tar包
```prettyprint
tar -zxvf git-2.8.3.tar.gz
cd git-2.8.3
```
3. 配置安装
```prettyprint
make configure
./configure prefix=/usr/local/git/
make && make install
git --version
```
4. 配置环境变量
```prettyprint
export PATH=$PATH:/usr/local/git/bin
```
<br><br>
### 本地创建公钥**[本地客户端]**
相当于一般网站注册账号
```prettyprint
ssh-keygen -t rsa
```
设置密码
![输入密码](https://user-images.githubusercontent.com/21979120/42792895-076846d6-89aa-11e8-94b0-8724f36195ea.png)
一路回车

将生成的id_rsa.pub文件里面的内容拷贝到服务端的**authorized_keys**文件
![id_rsa.pub](https://user-images.githubusercontent.com/21979120/42792976-6c52dfb6-89aa-11e8-97f0-13dd220ab0d0.png)

<br><br>
### 服务端储存公钥文件**[服务器]**
```prettyprint
adduser git
su - git
mkdir .ssh
chmod 700 .ssh
touch ~/.ssh/authorized_keys
chmod 644 ~/.ssh/authorized_keys
vi ~/.ssh/authorized_keys
```
把本地的``id_rsa.pub``里面的内容复制到``~/.ssh/authorized_keys``
<br><br>
### 禁用shell登陆**[可选]**
禁用后就无法 `su - git`了
找到git-shell目录
```prettyprint
[root@**** git]# which git-shell
/bin/git-shell
[root@**** git]# vi /etc/passwd
```
找到
```prettyprint
git:x:1001:1001:,,,:/home/git:/bin/bash
```
改为刚才which git-shell查到的目录`/bin/git-shell`：
```prettyprint
git:x:1001:1001:,,,:/home/git:/bin/git-shell
```
<br><br>
### 初始化git仓库项目
一般禁用了shell登陆以后创建git项目，我们可以这么操作：
假设仓库目录为：`/home/git-repository/test1`
创建一个空的git仓库，用本地代码覆盖远程目录
```prettyprint
cd /home/git-repository/test1
git init
mv .git /home/git-repository/test1
chown -R git /home/git-repository/test1
```
放弃远程仓库所有内容，用本地内容进行替换。
``` prettyprint
git remote add origin <url>
git push --force --set-upstream origin master
```
<br><br>
### 可能遇到的坑[异常]
1. 在执行本地替换远程目录时git remote add可能会有异常：`fatal: remote origin already exists`
解决方案：
```prettyprint
vi .git/config
```
修改url为正确的即可
<br>
2. `index-pack abnormal exit`这个问题有很多，需要具体问题具体分析，我遇到的是`Permission denied`没有操作目录得权限
解决方案：
```prettyprint
chown -R git /home/git-repository/test1
```
