---
title: Git强制推送
date: 2018-07-12 09:16:50
categories : Git
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
