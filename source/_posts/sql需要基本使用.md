---
title: sql需要基本使用
date: 2018-04-01 21:05:03
tags:
    - sql
---
## 函数

* decode(col, codition, rt1, rt2): 如果与codition相同返回结果1否则返回结果2
`decode(to_char(app_work_time, 'YYYY-MM-DD') , '2013-10-14', to_char(app_work_time, 'YYYY-MM-DD'), 0)`
* nvl(col, '')： 判断当前数据是否为空

## 最简单的创表语句
```sql
create table moudlename.tablename(
    id varchar(64)
)
```
## 数据库操作
```sql
-- 创建表
    create table student(
    id int primary key identity(1,1),
    stuName varchar(20),
    passsword varchar(20)
    )
--增
    insert into tjfx.student values(4, 'dj', '123')
    --从其他表插入
    insert into  jkxx.t_devrecord_equipmenttest(id, equipid, equipname)  select top 90 guid, equip_id, equip_name from equip.t_equipinfo;
-- 删
    delete tjfx.student where id = 3
-- 改
    update tjfx.student set stuName = 'dj1'
-- 查
    select * from tjfx.student
```

```sql

-- 创建增删改查
create table sample.test(
    id int primary key,
    occur_time timestamp
)

insert into sample.test values(1, now())

select * from sample.test

select sysdate(), now() from dual

select substr(now(), 1, 10) from dual

delete from sample.test where to_char(occur_time, 'YYYY-MM-DD')  = to_char(now(), 'YYYY-MM-DD')

```

## 触发器

    DML触发器分为：

    1、 after触发器（之后触发）

        a、 insert触发器

        b、 update触发器

        c、 delete触发器

    create trigger update_tigger on tjfx.student  after update
    as
    begin
            print '数据更新了！'
    end
    存储过程
    create proc proc_getStudentRecord(
        @id int, --默认输入参数
        @name varchar(20) out, --输出参数
        @age varchar(20) output--输入输出参数
    )
    as
        select @name = name, @age = age  from student where id = @id and sex = @age;

## 视图

    CREATE VIEW view_name AS
    SELECT column_name(s)
    FROM table_name
    WHERE condition


## 存储过程

    create procedure p_name
    @pageSize int,
    @page int,
    @tableName varchar(10)
    as
    declare @temp int
    set @temp=@pageSize*(@page - 1)
    begin
     select top (@pageSize) * from @tableName where gId not in (select top (@temp) gId from t_goods) order by gId
    end

## 函数创建

```sql
CREATE OR REPLACE FUNCTION "emsetl"."GUID" return varchar(64)
as
    str varchar(64):=guid();
begin
  return str;
end;
```

## 索引

    普通索引 ALTER TABLE `table_name` ADD INDEX index_name ( `column` )
    主键索引 ALTER TABLE `table_name` ADD PRIMARY KEY ( `column` )
    唯一索引 ALTER TABLE `table_name` ADD UNIQUE ( `column` )
    全文索引 ALTER TABLE `table_name` ADD FULLTEXT ( `column`)
    如何添加多列索引 ALTER TABLE `table_name` ADD INDEX index_name ( `column1`, `column2`, `column3` )

## sql执行顺序

    8. SELECT (9)DISTINCT  (11)<Top Num> <select list>
    1. FROM [left_table]
    3. <join_type> JOIN <right_table>
    2. ON <join_condition>
    4. WHERE <where_condition>
    5. GROUP BY <group_by_list>
    6. WITH <CUBE | RollUP>
    7. HAVING <having_condition>
    10. ORDER BY <order_by_list>


## 数据库常用操作



* 包含这列的所有数据库表名
    SELECT table_name FROM all_col_comments WHERE column_name LIKE '%A%';


* 数据库插入列
    alter   table   表名   add   列名   数据类型
    如：alter   table   student   add   nickname   char(20)

* 数据库删除列
    ALTER TABLE 表名 DROP CONSTRAINT 默认约束名
    GO
    ALTER TABLE 表名   DROP COLUMN	字段名
    GO

* 外连接
    right join  表 on a表.字段 = b表.字段 右连存右 左连存左
    full outer join 全外连接

* 避免拼接的时候没有where语句
    开头加上where 1=1



* 把查询表当作新表
    1.
    select a.filed, b.num
    from  table_name a,(select count(*) as num from table_name b

    2.
    with 别名 as 数据库查询语句
    with b as
            (
                    select count(*) as num from table1_name
            )
    select a.filed, b.num from table2_name a,b

* 虚表
    select '是','否',Date() from dual



* 时间比较
    --and occur_time Between '2016-06-10 17:03:00' And '2016-06-10 17:03:00'
    --and datediff(day,'2016-05-10','2016-05-11')
    --and occur_time > '2016-05-10'.toDate() and occur_time < '2016-05-11'.toDate()
    and to_date(occur_time, 'yyyy-mm-dd')>= to_date('2016-06-01', 'yyyy-mm-dd')
    and to_date(occur_time, 'yyyy-mm-dd')<= to_date('2016-06-01', 'yyyy-mm-dd')




* 给数据库表添加索引
ALTER TABLE `table_name` ADD INDEX index_name ( `column` )

## 数据操作注意事项

* 删除表的时候先删除外层再删除里层
* identity(1,1)表自动增长
* 外模式-视图   模式-表 内模式-文件
* 修改表
alter table tablename alter column colname newDataType
比如：alter table mytable alter column mycol1 int default 0
* 实体设置编号 联系不用设置编号

## 表的行级拼接与列级拼接


* 行级拼接union all

select * from A where .....

union all

select * from A where ......


* 列级拼接union all

select (select * as ... from A where .....) as ..., (select * as .. from B where ......) as ....
from dual

* 数据库连接符号

||

## case when 的两种写法


select case 列名 when 条件 then 结果 when .. then ..  end
select case when 条件 then 结果 when .. then .. end


## 聚集函数

    sum          求和
    count        记录条数
    avg          平均

## 普通函数


    nvl(字段, 0) 判断是否为空如果为空就置0
    to_char(date, [fmt]) 时间转字符串
    to_date('1996-01-02') 字符串转日期

## 数据库查询语句

1. 原样查出，然后需要转义的转义前后的数据都保留
2. 查询字段也保留

> mysql 查询语句

```mysql
SELECT * FROM `t_test`;

update t_test set col5 = 'B'

create procedure setA()
begin
update t_test set col5 = 'A';
end;

create procedure setFlagById(in n int, in c char)
begin
update t_test set col5 = c where col1 = n;
end;


drop procedure setFlagById;

call setFlagById(2, 'c');
```
