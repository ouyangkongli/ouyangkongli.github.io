---
layout: post
title: oracle sql 基础
description:  oracle sql 基础
tag: [sql, oracle]
comments: true
categories: oracle
---

### oracle 常用系统函数

{% highlight sql %}
 --常用系统函数
        --String
        select ascii('A') A,ascii('a') a,ascii('0') zero, ascii(' ') space from dual;  --字符对应的ascii码

        select chr(90), chr(72) from dual; -- ascii码对应的字符

        select concat('hello ','oracle') from dual; --字符串连接

        select initcap('oh my god') from dual; --每个单词的首字母大写

        select instr('oracle 11g','1',3,2) from dual; --instr(s1,s2,i,j)返回字符串s2在s1中第j次出现时的位置，从s1的第i个字符开始

        select ltrim('###left###','#'),rtrim('###right    '),trim('#' from '###trim###') from dual;

        select replace('Always gonna be an test uphill battle', 'test', '') from dual;


        --Date
        select add_months(sysdate, 2) from dual;
        select LAST_DAY(sysdate) from dual;
        select months_between(sysdate+30, sysdate) from dual;


        --convert
        select sysdate as defaultDateFormat, to_char(sysdate, 'YYYY-MM-DD')  from dual; 

{% endhighlight %}

       
<!-- more -->
