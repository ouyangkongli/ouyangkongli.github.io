---
layout: post
title: ibatis insert 
dscription:  ibatis insert 
tag: [ibatis]
comments: true
categories: blog
---

使用Ibatis时，常常会碰到这样一个场景：

Java程序中有一个对象data，现在需要把data插入数据库中，而这条数据有一个字段A是在数据库中生成（例如oracle中的sequence），并且insert数据之后，需要立即返回这个数据(带有A字段)。现在有2个解决方案：

1. 首先在java代码中做一次db call，拿到A.nextval，再把这个值set到data中; 然后再做一次db call (把data插入到db).  我们目前的项目普遍采用这个做法，不得不吐槽一下，这是个相当糟糕的做法(插入1条数据，而做了2次db call).

2. 利用Ibatis中的SelectKey.

<!-- more -->

下面介绍一下SelectKey.


|属性     |描述      |
|---------|---------|
|keyProperty        |selectKey 语句结果应该被设置的目标属性。    |
|resultType        |结果的类型，IBatis 允许任何简单类型用作主键的类型,包括字符串。   |
|order        |这可以被设置为 BEFORE 或 AFTER。如果设置为 BEFORE,那么它会首先选择主键,设置 keyProperty 然后执行插入语句。如果设置为 AFTER,那么先执行插入语句,然后是 selectKey 元素-这和如 Oracle 数据库相似,可以在插入语句中嵌入序列调用。    |


```sql
<insert id="insertUser" parameterClass="ibatis.User"> 
    <selectKey resultClass="long" keyProperty="id"> 
        select SEQ_USER_ID.nextval as id from dual 
    </selectKey> 
        insert into user(id,name,password) 
        values (#id#,#name#,#password#) 
</insert> 
```

该句话执行完之后，传进来的参数User对象DO里的id字段就会被赋值成sequence的值。 

当然，如果你insert data之后，不需要data的字段A，那么的sql完全可用这么写：

```sql
<insert id="insertUser" parameterClass="ibatis.User"> 
    insert into user(id,name,password) 
    values(SEQ_USER_ID.nextval,#name#,#password#) 
</insert> 
```