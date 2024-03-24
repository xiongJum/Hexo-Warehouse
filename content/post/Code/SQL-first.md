---
title: SQL 基础其一
date: 2020-07-22 15:24:00
tags: ["SQL"]
categories: ["编程"]
---

### 查询

##### 基本查询语句

~~~sql
-- select (列名1, 列名2 或 *) from 表名 where 条件
select order_no, trip_no from trip_order where order_no > '20191009012100000013';
~~~
<!--more-->

+ 逻辑运算符 如 and、in 等

~~~sql
-- select 列名1, 列名2 from 表名 where 条件 (and 或 or 或 in) 条件
select order_no, trip_no, trade_amount from trip_order where order_no > '20191009012100000013' and trade_amount = 100;
select order_no, trip_no, trade_amount, state from trip_order where state = 2 or trade_amount = 100;
select order_no, trip_no, trade_amount, state from trip_order where order_no > '2019100901' and (state = 2 or trade_amount = 100);
-- in
select order_no, trip_no, trade_amount, state from trip_order where state in (2, -3);
select order_no, trip_no, trade_amount, state from trip_order where state not in (2, -3); -- 查找不包含 2,3的数据
-- like
select order_no, trip_no, trade_amount, state from trip_order where order_no like '201906%' -- 匹配以201906开头的数据
select order_no, trip_no, trade_amount, state from trip_order where order_no like '%201906%' -- 匹配中间包含201906的数据
select order_no, trip_no, trade_amount, state from trip_order where order_no like '%201906' -- 匹配以201906结尾的数据
~~~

+ 基本分组语句

~~~sql
-- select 列名1 或 from 表名 group by 列名1;
select state from trip_order group by state;
-- 就结果而言和去重所查询的结果一致 distinct
select distinct state from trip_order;
~~~

~~~sql
-- 统计每个分组的数据数量, count()
select state, count(1) from trip_order group by state;
-- 查看数量为某个区间的分组, having
select state, count(1) from trip_order group by state having count(1) > 200;
~~~

##### 子查询

+ 嵌套在另一个查询中的查询
  + 放在括号内的查询成为子查询，也被称为内部查询或内部选择
  + 包含子查询的查询称为外部查询或外部选择

~~~sql
-- select 列名3 from (select 列名2, 列名3 from 表名 where 条件) 别名 where 条件;
-- 一般子查询是跨表查询 本次示例展示的是相同表之间使用子查询
select state, out_order_no from (select order_no, state, out_order_no from trip_order where order_no > '20191009012100000013') t where state in (2, 3);
select state, trip_no from trip_order state not in (select state from trip_order where state = 2);
~~~

##### 表连接

+ 内连接
  + 关键词 inner join ... on
  + 只会关联并展示两张表中都存在的数据

~~~sql
select tr.id, tr.merchant_no, trq.pos_transaction_bill_no 
from ticket_user_driving_record as tr
inner join ticket_user_driving_record_detail_qrcode as trq 
on tr.id = trq.record_id;
~~~

+ 左连接
  + 关键词 left join ... on
  + 以主表为主，展示所有主表的行数据，若关联的表没有匹配则展示null

~~~sql
select tr.id, tr.merchant_no, trq.pos_transaction_bill_no 
from ticket_user_driving_record as tr
left join ticket_user_driving_record_detail_qrcode as trq 
on tr.id = trq.record_id;
~~~

+ 右连接
  + 关键词 right join ... on 
  + 和左连接相反，以关联的表为主，会展示所有关联表中的行数据
+ 全外连接
  + 关键词 full join ...on
  + 展示全部的行数
+ 多表连接
  + 超过两张表的表连接

~~~sql
select tr.id, tr.merchant_no, trq.pos_transaction_bill_no 
from ticket_user_driving_record as tr
inner join ticket_user_driving_record_detail_qrcode as trq 
on tr.id = trq.record_id
inner join ticket_user_driving_original_record_qrcode as toq
on tr.id = toq.record_id
~~~



### 插入数据

~~~sql
-- insert into 表名 (列名1, 列名2) values (插入的数据1, 插入的数据2)
insert into trip_order (state trip_no) values (3, '20191009012100000013');
~~~

### 更新数据

~~~sql
-- update 表名 set 更新的数据 where 条件
update trip_order set state = 2 where trip_no = '20191009012100000013';
~~~

### 删除数据

~~~sql
-- delete from 表名 where 条件
delete from trip_order where trip_no = '20191009012100000013';
~~~





