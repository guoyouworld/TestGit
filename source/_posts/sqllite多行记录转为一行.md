---
title: sqllite多行记录转为一行
date: 2018-07-20 09:30:47
tags: [sql,sqllite]
---
sqllite多行记录转为一行...
<!--more-->
### 实现效果
|  column1        |  column2  |
| :--------   | :-----  |
|1|中|
|1|国|
|1|人|
|2|外|
|2|国|
|2|人|

实现下面这种效果：

|  column1        |  column2  |
| :--------   | :-----  |
|1|中,国,人|
|2|外,国,人|

<br><br>
### 关键字：
`group_concat`将group by产生的同一个分组中的值连接起来，返回一个字符串结果.
group_concat( 连接的字段,'分隔符' )<sub>分隔符默认为逗号，可以不写</sub>
可以在**连接的字段**前加distinct除重

```prettyprint
select column1,group_concat(distinct column2) from test group by column1;
```
