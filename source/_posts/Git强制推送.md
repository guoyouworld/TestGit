---
title: Git强制推送
date: 2018-07-12 09:16:50
tags: [git,github]
toc: true
---
如何替换远程仓库内容？
如何替换本地仓库修改？
<!--more-->
### 远程仓库替换
放弃远程仓库所有内容，用本地内容进行替换。
``` prettyprint
git remote add origin <url>
git push --force --set-upstream origin master
```
<br>
### 本地仓库替换
放弃本地仓库所有修改，用远程内容进行替换。
``` prettyprint
git reset --hard HEAD  
git pull
```
<br><br>

### 后续的坑
强制替换远程仓库的时候如果添加了readme.md文件
那么在执行替换命令的时候会提示 `error: failed to push some refs to`
因为远程仓库与本地文件不一致
这时候需要：
``` prettyprint
git init
git remote add origin https://github.com/guoyouworld/ImageManagement.git
git pull origin master
git push --force --set-upstream origin master
```

### 微信公众号
![wechat](https://user-images.githubusercontent.com/21979120/43175494-eabdbb26-8ff1-11e8-8c08-5309d9f5848c.png)
