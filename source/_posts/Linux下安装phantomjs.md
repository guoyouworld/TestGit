---
title: Linux下安装phantomjs
date: 2018-07-17 14:12:15
tags: [Linux,爬虫,phantomjs]
toc: true
---
Linux下安装phantomjs...
<!--more-->
### 下载
[官网下载：http://phantomjs.org/download.html](http://phantomjs.org/download.html)

### 安装
解压-->移动到目录-->配置环境变量
```prettyprint
tar  -jxvf  phantomjs-2.1.1-linux-x86_64.tar.bz2
mv phantomjs-2.1.1-linux-x86_64 /opt/phantomjs
export PATH=$PATH:/opt/phantomjs/bin
```
### 代码调用
java代码：
```prettyprint
...
System.setProperty("phantomjs.binary.path", "/opt/phantomjs/bin/phantomjs");
...
```
<br><br>
### 微信公众号
![wechat](https://user-images.githubusercontent.com/21979120/43175494-eabdbb26-8ff1-11e8-8c08-5309d9f5848c.png)
