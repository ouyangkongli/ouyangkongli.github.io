---
layout: post
keywords: mysql
description:
title: "mysql 常用命令"
categories: [mysql]
tags: [mysql]
group: archive
icon: file-alt
---
{% include oukongli/setup %}

###mysql服务的启动与停止
{% highlight cpp %}
net start mysql
net stop mysql
{% endhighlight %}


###登陆mysql
用法如下：

-  `mysql -u username -p`  
-  `password`  

注意：若要链接到另外的机器，则需要计入一个参数`-h ip`

###增加新用户
格式：grant 权限 on 数据库.* to 用户名@登录主机 identified by "密码"  
如，增加一个用户user1密码为password1，让其可以在本机上登录， 并对所有数  据库有查询、插入、修改、删除的权限。首先用以root用户连入mysql，然后键入以下命令：
`grant select,insert,update,delete on *.* to user1@localhost Identified by "password1";`  
如果希望该用户能够在任何机器上登陆mysql，则将localhost改为"%"。  
如果你不想user1有密码，可以再打一个命令将密码去掉。`grant select,insert,update,delete on mydb.* to user1@localhost identified by "";`

###操作数据库
登录到mysql中，然后在mysql的提示符下运行下列命令，每个命令以分号结束。  

<!-- more -->

{% highlight cpp %}
1、 显示数据库列表。
show databases;
缺省有两个数据库：mysql和test。 mysql库存放着mysql的系统和用户权限信息，我们改密码和新增用户，实际上就是对这个库进行操作。
2、 显示库中的数据表：
   use mysql;
   show tables;
3、 显示数据表的结构：
    describe 表名;
4、 建库与删库：
   create database 库名;
   drop database 库名;
5、 建表：
   use 库名;
   create table 表名(字段列表);
   drop table 表名;
6、 清空表中记录：
   delete from 表名;
   truncate 表名;（效率更高）
7、 显示表中的记录：
  select * from 表名;
{% endhighlight %}

###常用查询
{% highlight cpp %}
like 表示模糊查询
   select * from table where column like 'x%';
in 表示某个值在某个数组内
   select * from table where column in (2,1);
between 表示可以查询某个范围内的数据;
   select * from table where column between '1989-08-21' and '1990-10-10';
查询空值
   select * from table where column is null;
排序
   select * from table order by column;
常用函数
   max()   min()  count()
分组
   select * from table group by column1, column2;
having
   select cla_id,count(id) as pn from t_stu whereid is not null group by cla_id having pn>50;
跨表查询
   select * from t_cla t1, t_stu t2 where t1.id=t2.cla_id;
   或者
   select * from t_cla t1 join t_stu t2 on (t1.id = t2.cla_id);(常用)  
分页查询
   selecat * from table limit a,b;  
{% endhighlight %}

