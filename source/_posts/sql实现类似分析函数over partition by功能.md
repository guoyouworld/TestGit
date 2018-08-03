---
title: sql实现类似分析函数over partition by功能
date: 2018-07-13 10:26:02
tags: [sql,sqlite]
---
近期项目用到了类似oracle的分析函数over partition by功能，但是我们用的数据库是sqllite，并不支持分析函数。
So ......
<!--more-->
### 明确需求
我们需要取所有数据，如果某一字段有重复从字段重复数据内取一条时间最近接近现在时间的.
> * (假设id重复)
> * (假设存时间的字段为dtime)
> * (假设表明为tableName)

**在oracle内实现：**
```prettyprint
select * from (
select a.*,
row_number() over(partition by id order by dtime desc) rn
from tableName
)
where rn =1;
```
<br>

**类似的功能在没有分析函数的数据库：**
```prettyprint
select *,
(select count(1) from tableName b where id=b.id and dtime<b.dtime ) rn
from tableName
where rn=0
```
<br><br>
### 微信公众号
![wechat](https://user-images.githubusercontent.com/21979120/43175494-eabdbb26-8ff1-11e8-8c08-5309d9f5848c.png)
