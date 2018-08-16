---
title: sql取一组数据
date: 2018-08-16 15:31:54
tags: [sql,sqlite,mysql]
---
sql取一组数据...
<!--more-->

**前提条件是：column1 是`distinct`的**

### 实现效果
```prettyprint
select column1,column2 from A;
```
|  column1        |  column2  |
| :--------   | :-----  |
|***|1|
|***|1|
|***|2|
|***|2|
|***|2|
|***|3|
|***|4|
|***|4|
|***|4|

实现下面这种效果：

|  column1        |  column2  |
| :--------   | :-----  |
|***|4|
|***|4|
|***|4|

<br><br>

### 实现1
如果查询语句带有多个where条件，则实现1就不合适了。
需要在子查询中添加多个相同的where条件。

```prettyprint
select column1,column2 from A where column2=(select max(column2) from A);
```

<br><br>

### 实现2
不需要考虑主查询的where条件

```prettyprint
select column1,column2,(select max(column2) from A a where a.column1=column1) max
from A where column2=max;
```


个人不成熟见解，如有疑问请及时联系。
<br><br>
### 微信公众号
![wechat](https://user-images.githubusercontent.com/21979120/43175494-eabdbb26-8ff1-11e8-8c08-5309d9f5848c.png)
