---
title: Spring源码深度解析[持续更新中...]
date: 2018-07-13 13:53:00
tags: [Java,Spring]
toc: true
---
Spring源码深度解析读书笔记...
<!--more-->
### Spring的整体架构
![Spring的整体架构](https://user-images.githubusercontent.com/21979120/42675065-e926e4dc-86a4-11e8-9f6c-38950ea6e1fa.png)


### Spring环境搭建
到你想要储存源码的目录执行下面的语句（git安装不用讲了吧）
```prettyprint
git clone https://github.com/spring-projects/spring-framework.git
```
实在不会Git可以到[Spring托管网站](https://github.com/spring-projects/spring-framework)去下载。

下载完成后,到想要学习的目录执行下面的命令（例如：spring-core目录）：
```prettyprint
gradle cleanidea eclipse
```
然后导入eclipse...
***
额，书上是这么说的，我去试试...
坑啊：`bash: gradle: command not found`
...
回过头来发现，需要安装一个gradle构建工具
安装地址：[https://gradle.org/releases/](https://gradle.org/releases/)
![下载位置](https://user-images.githubusercontent.com/21979120/42677677-10c2b674-86af-11e8-873d-ad189795f938.png)
**配置环境变量：**
GRADLE_HOME=D:\gradle\gradle-4.8.1-all\gradle-4.8.1
%GRADLE_HOME%\bin;
<sub>配置完记得重启git，要不然还是找不到命令，别问我是怎么知道的....</sub>

![failed](https://user-images.githubusercontent.com/21979120/42678556-d64d6f36-86b1-11e8-8ada-abf9c0374de8.png)
哇，心态炸了，不写了...
