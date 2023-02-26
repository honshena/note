---
title: 第2章--数据库语言(CRUD--增删改查)
date: 2021/3/8 22:10:45
categories:
- [Mysql]
tags:
- [Mysql]
- [数据库]
- [后端]
- [笔记]
---
# 数据库语言(CRUD--增删改查)

# DDL--定义语言
**所有的创建和删除应该加上判断`if [not] exists`所有的标识符应该使用反引号包裹&#96;&#96;**

## 数据库

### 创建数据库

```cmd
create database [if not exists] 数据库名;
```

### 删除数据库

```cmd
drop database [if exists] 数据库名;
```

### 使用数据库

```cmd
use 数据库名;
```

### 数据库引擎

- innodb

- myisam

  ![数据库引擎区别](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-03-13_00-17-56.png)

  ![区别](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-03-13_00-18-13.png)

## 表

### 表数据类型

1. 数值
   - tinyint: 小数据(1字节)
   - smallint: 较小数据(2字节)
   - mediumint: 中等大小数据(3字节)
   - int: 标准整数数据(常用4字节)
   - bigint: 较大的数据(8字节)
   - float: 单精度浮点数(4字节)
   - double: 双精度浮点数(8字节)
   - decimal: 字符串浮点数(金融计算中常用)
2. 字符串
   - char: 字符串 (0~255个字符)
   - varchar: 可变长字符串(0~65535字符常用)
   - tenytext: 微型文本(2^8-1个字符博客常用)
   - text: 文本串(2^16-1大型文本书籍常用)
3. 时间日期
   - date: YYYY-MM-DD, 日期
   - time:  HH:MM:SS ,时间
   - datetime: YYYY-MM-DD HH:MM:SS 最常用的时间格式
   - timestamp: 时间戳 1970.1.1至今的毫秒数,常用
   - year: 年份
4. null
   - 空值,未知不要使用null值进行计算,结果都是null没有意义

### 字段属性

![字段属性](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-03-10_12-45-23.png)

1. unsigned
   - 无符号整数,不能声明为负数
2. zerofill
   - 声明该字段为空时0填充
   - 不足的位数使用0填充
3. 自增(auto_increment)
   - 自动在上一条记录上自增
   - 通常用来设置唯一的主键index,而且必须是整数
   - 可以自定义主键自增的起始值和步长
4. 非空(null和not null)
   - 赋值为空报错
5. 默认(default)
   - 不指定该列值则为默认值

**每一张表都必须存在以下五个字段,表示该表存在的意义(阿里巴巴规范) **

```mysql
id 主键
version 乐观锁
is_delete 伪删除
gmt_create 创建时间
gmt_update 修改时间
```
### 创建表

```cmd
create table [if not exists] `表`(
	`列名` 类型(位数) [字段属性] [索引] [注释],
	`列名` 类型(位数) [字段属性]
)engine=引擎 default charset=默认编码 comment='注释'
```

**这里需要说明一下: mysql默认字符编码是latin1不支持中文字符,所以需要在初始化时设置编码字符集或者修改my.ini文件的默认编码`character-set-server=utf8`建议创建表时设置字符集编码**

举个栗子

```mysql
create table if not exists `student`(
	`id` int(4) not null auto_increment comment 'sutdent id',
	`name` varchar(20) not null  comment 'student name',
    `sex` varchar(2) not null default '0' comment 'student sex',
    `pwd` varchar(20) default null comment 'login password',
    primary key (`id`)
)engine=innodb default charset=utf8 comment='student table'
```

tips: 表和字段名使用&#96;包裹起来,括号必须是英文的半角: (),注释(comment)字段必须是单引号''或双引号"",每一列最后加逗号,最后一列不用加逗号,一张表只有一个主键,主键也可以在列名后的字段属性中添加`primary key`一般不建议而是选择在最后设置主键便于阅览**表的名字在sqlyog中是黑色,comment是红色高亮,且正确的标识符会被sqlyog替换为大写蓝紫色**执行成功后刷新表

### 删除表

```mysql
drop table [if exists] 表
```

### 查看创建数据库、表

```mysql
show create table `表` -- 查看表的创建
show create database `库` -- 查看库的创建
```

### 查看表结构

```mysql
desc 表;
```

### 操作表字段属性

#### 修改

```mysql
alter table 表1 rename as 表2; -- 将表1的名字修改为表2
alter table 表 add 字段 类型(大小) 属性; -- 给表增加字段: alter table stu add age int(11);
alter table 表 modify 字段 类型(大小) 属性; -- 修改表的字段: alter table stu modify age varchar(11);
alter table 表 change 字段1 字段2; -- 修改表字段的名称,字段重命名
alter table 表 drop 字段; -- 删除字段
```

tips: change用来重命名,modify用来修改约束

#### 添加外键

```mysql
alter table 表 
add constraint `FK_主表外键名` foreign key `主表外键名` references `从表名`(`从表键名`);
```

# DML--操作语言

## 外键

```mysql
CREATE TABLE `grade`(
`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
PRIMARY KEY (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

-- 学生表的gradeid字段要去引用年级表的gradeid
-- 定义外键key
-- 给这个外键添加约束(执行引用) reference引用

CREATE TABLE IF NOT EXISTS `student`(
`id` INT(10) NOT NULL AUTO_INCREMENT COMMENT '学号',
`name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
`pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
`sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
`birthday` DATETIME DEFAULT NULL COMMENT'出生日期',
`gradeid` INT(10) NOT NULL COMMENT '学生的年级',
`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
PRIMARY KEY(`id`),
KEY `FK_gradeid` (`gradeid`),
CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade`(gradeid)
)ENGINE=INNODB DEFAULT CHARSET=utf8
```

**删除外键表需要先删除引用外键的表,以上是物理外键~数据库级别的外键不建议使用建议使用程序去查多张表**

## 增删改

### 增

```mysql
-- 插入一行
insert into 表 (`字段1`,`字段2`,`字段3`...) values (值1,值2,值3...);
-- 自增可以省略值,如果不写表的字段mysql会一一从左向右匹配例如: INSERT INTO `stu` VALUES(1,'java',1,'123456');
insert into 表 values (值1,值2,值3...);
-- 插入多行
insert into 表 (`字段1`,`字段2`,`字段3`...) values (值1,值2,值3...),(值1,值2,值3...);
insert into 表 values (值1,值2,值3...),(值1,值2,值3...),(值1,值2,值3...)...;
```

### 删

```mysql
delete from 表 [where 条件] -- 删除某一行数据,不加条件就是删库跑路,不会影响自增
truncate 表 -- 清空一张表,表的索引和结构不会变,会重置自增例计数器清0
```

![不同点](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-03-15_13-17-54.png)

### 改

```mysql
update 表 set `字段`=值 WHERE `字段`=值; -- 例如: UPDATE `stu` SET `name`='ad' WHERE `id`=1;如果不指定条件则会修改所有表下的值
update 表 set `字段1`=值1,`字段2`=值2... WHERE `字段`=值; -- 修改多个值
-- value不一定是定值也可以是变量
```

#### where运算符

```mysql
< 小于
= 等于
> 大于
<> 或 != 不等于
<= 小于等于
>= 大于等于
between num1 and num2 在某个范围内
and 与
or 或
-- where `name`='honshen' and `sex`=1;
```

![示列](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-03-16_13-08-32.png)

# DQL--查询语言

```mysql
SELECT [ALL | DISTINCT]
{ * | table.* | [table.field1[as alias1][,table.field2[as alias2]][,...]]}
FROM table_name [as tab1e_alias]
[left | right | inner join table_name2] -- 联合查询
[WHERE ...] -- 指定结果需满足的条件
[GROUP BY ...] -- 指定结果按照哪几个字段来分组
[HAVING] -- 过滤分组的记录必须满足的次要条件,条件和where一样
[ORDER BY ...] -- 指定查询记录按一个或多个条件排序
[LIMIT {[offset,]row_count | row_countOFFSET offset}]; -- 指定查询的记录从哪条至哪条
-- 其中[]表示可选, {}表示必选 表或字段都可以取别名
```



## 条件查询
- select 字段名 from 表名 where 条件；
   	> 运算符
    >  \>、<、<=、>=、=、<> : <>在 SQL 中表示不等于，在 mysql 中也可以使用!=,没有==
    > between...and:在一个范围之内，如： between 100 and 200相当于条件在 100 到 200 之间，包头又包尾
    > in(集合):集合表示多个值，使用逗号分隔
    > like'张%':模糊查询，以张开头的数据
    > like'%张':模糊查询，以张结尾的数据
    > like'%张%':模糊查询，中间含张的数据
    > like'_张'：第二个字为张的两个字的数据
    > is null:查询某一列为 NULL 的值，注：不能写=NULL
- 逻辑运算符
    > and 或 && : 与,SQL 中建议使用前者，后者并不通用。
    > or 或 || : 或。
    > not 或 ! : 非。
- in关键字
    > SELECT 字段名 FROM 表名 WHERE 字段 in (数据 1, 数据 2...);
    > in 里面的每个数据都会作为一次条件，只要满足条件的就会显示
- 范围查询
    > BETWEEN 值 1 AND 值 2
    > 表示从值 1 到值 2 范围，包头又包尾
    > 比如： age BETWEEN 80 AND 100 相当于： age>=80 && age<=100
- like关键字
   > LIKE 表示模糊查询
   > SELECT * FROM 表名 WHERE 字段名 LIKE '通配符字符串';
   > MySQL通配符
   > % ： 匹配任意多个字符串
   > _ ： 匹配一个字符

## 指定查询字段

```mysql
select * from 表 -- 查询所有字段
-- 查询指定字段
select `字段1`,`字段2`,`字段3` from 表
-- 给结果起别名
select `字段1` as 别名1,`字段2` as 别名2,`字段3` as 别名3 from 表 as 别名 -- 可以给字段或者表起别名
select concat('字符串', 字段名) from student -- 给结果前加上字符串,concat是字符串拼接函数
select @@auto_increment_increment -- 查询自增的步长
select `字段1`+1,`字段2`*2 from 表 -- 对查询结果做运算
```

**select进阶**

```mysql
select 表达式 from 表 -- 表达式可以是: 文本值,函数式,计算表达式,列(字段),系统变量
-- 常用函数
select abs(-9)  -- 9 绝对值
select ceiling(9.2) -- 10 向上取整
select floor(9.2) -- 9 向下取整
select rand() -- 返回0-1之间随机数
select sing(数字) -- 判断一个数的符号 负数返回-1 0返回0 正数返回1
select char_length("abc") -- 3 返回字符串长度
```

## mysql函数

常用函数: [https://www.runoob.com/mysql/mysql-functions.html](https://www.runoob.com/mysql/mysql-functions.html)

## 聚合函数

| 函数名  | 描述   |
| ------- | ------ |
| COUNT() | 计数   |
| SUM()   | 求和   |
| AVG()   | 平均   |
| MAX()   | 最大值 |
| MIN()   | 最小值 |
| ...     |        |

```mysql
select count(`student`) from stu; -- 查询stu表中有多少个student
-- count(列) 指定列,查询列的记录,会忽略所有的null值
-- count(*) 和指定列计数一样,但不会忽略null值,计数行数
-- count(1) 和指定列计数一样,但不会忽略null值,计算行数
-- 列名为主键时,count(列)比count(1)快
-- 列名不为主键时,count(1)比count(列)快

select sum(`score`) as 总和 from stu -- 查询总和
select avg(`score`) as 平均 from stu -- 查询平均
select max(`score`) as 最高 from stu -- 查询最高
select min(`score`) as 最低 from stu -- 查询最低
```

## 去重

```mysql
select distinct 字段 from 表 -- 去重查询
```

## 模糊查询

| 运算符      | 语法               | 描述                                                         |
| ----------- | ------------------ | ------------------------------------------------------------ |
| like        | a like b           | 如果a匹配b则为真(结合%和_使用,%代表0到任意个字符,_代表一个字符) |
| in          | a in (a1,a2,a2...) | a在a1,a2,a3中则为真                                          |
| is null     | a is null          | a为空则为真                                                  |
| is not null | a is not null      | a不为空则为真                                                |

```mysql
select 字段 from 表 where 字段 in (值1,值2,值3...); -- 精确匹配查询 字段中有这3个字符串值其中一个都能被查出
select 字段 from 表 where 字段 like '值_'; -- 值必须引号引出,引号中_表示只有一个,%表示0到多个,例如: %a%表示查询含有a的字符串
```

## 联表查询

七种jion理论:[https://blog.csdn.net/huang__2/article/details/83688001](https://blog.csdn.net/huang__2/article/details/83688001)

```mysql
-- 一定要确定交点才能联表查询
select 表1别名.字段1, 字段2, 字段3... from 表1 as 表1别名 inner join 表2 as 表2别名 on 表1别名.字段1=表2别名.字段1 where 条件-- 该字段是表1和表2都具有的,查并集
select 表1别名.字段1, 字段2, 字段3... from 表1 表1别名 right join 表2 表2别名 on 表1别名.字段1=表2别名.字段1 where 条件 -- 该字段是表1和表2都具有的,查右集
join 联接的表 on 判断条件 -- 是联接查询
where -- 等值查询
-- 三表查询实例
select s.studentNo, studentname, subjectNo
from student s
right join result r
on r.studentNo = s.studentNo
inner jion subjets sub
on r.subjectNo = sub.subjectNo
```

| inner join | 如果表中至少有一个匹配,就返回值       |
| ---------- | ------------------------------------- |
| left jion  | 从左表返回所有的值,即时右表没有匹配   |
| right join | 从右表中返回所有的值.即时左表没有匹配 |

## 自连接

**核心:一张表拆为两张**

```mysql
select 字段1 as 别名1 , 字段2 as 别名2
from 表 as 别名1, 表 as 别名2 -- 两张表是同一个
where 别名1.字段1别名 = 别名2.字段2别名 -- 一般是这样
```

## 分页和排序

```mysql
select 字段1 as 别名1 , 字段2 as 别名2
from 表 as 别名1, 表 as 别名2 -- 两张表是同一个
where 别名1.字段1别名 = 别名2.字段2别名 -- 一般是这样
order by `字段` asc -- 给定字段升序 降序:desc
limit 数字1,数字2 -- 分页,该语句放在最后,数字1是该页数据的起始位置,数字2是该页的数据条数
```

![分页](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-03-19_20-23-46.png)

## 子查询和嵌套查询

### 子查询

**where语句中条件计算得到--在where中嵌套查询语句**

```mysql
select 字段 from 表
where 条件中嵌套查询 
-- 例如
select id from `usr1`
where id = (
	select id from `usr2` where id<100
)
```

## md5数据库级别的加密

**使用函数md5()对数据库的字段加密后更新**

```mysql
update stu set pwd=MD5(pwd) -- 更新stu表中的密码
insert stu values (字段1,字段2,...,MD5('密码')) -- 插入时加密
select * from stu where name='xiaohong' and=MD5('123456') -- 校验时加密
```

## 事务

**将一组sql放在一个批次去执行,要么都成功要么都失败**

**mysql事务是默认提交的**

> 事务原则: (ACID) 原子性,一致性,隔离性,持久性 (脏读,幻读,不可重复读) 
>
> 原子性: 一个事务要么全部提交成功，要么全部失败回滚，不能只执行其中的一部分操作，这就是事务的原子性
>
> 一致性: 事务的执行不能破坏数据库数据的完整性和一致性，一个事务在执行之前和执行之后，数据库都必须处于一致性状态。如果数据库系统在运行过程中发生故障，有些事务尚未完成就被迫中断，这些未完成的事务对数据库所作的修改有一部分已写入物理数据库，这是数据库就处于一种不正确的状态，也就是不一致的状态
>
> 隔离性: 事务的隔离性是指在并发环境中，并发的事务时相互隔离的，一个事务的执行不能不被其他事务干扰。不同的事务并发操作相同的数据时，每个事务都有各自完成的数据空间，即一个事务内部的操作及使用的数据对其他并发事务时隔离的，并发执行的各个事务之间不能相互干扰。
>
> 持久性: 一旦事务提交，那么它对数据库中的对应数据的状态的变更就会永久保存到数据库中。--即使发生系统崩溃或机器宕机等故障，只要数据库能够重新启动，那么一定能够将其恢复到事务成功结束的状态
>
> 脏读: 一个事务读取了另一个事务未提交的数据
>
> 不可重复读: 一个事务多次读取结果不一致
>
> 幻读: 一个事务读取到了另一个事务插入的数据

```mysql
set autocommit = 0 -- 关闭事务的自动提交autocommit = 1开启

-- 手动处理事务
set autocommit = 0 
start transaction -- 标记事务的开始
-- 这里写sql语句,例如
insert xxxx
insert xxxx
savepoint 保存点名称 -- 设置保存点
-- 提交
commit
-- 回滚
rollback to savepoint 保存点名称 -- 回滚到保存点,
release savepoint 保存点名称 -- 删除保存点
-- 事务结束
set autocommit = 1 -- 开启事务自动提交
```

## 索引

**索引是一种数据结构**

分类:

 	1. 主键索引: `primary key`
 	 - 主键不可重复,是唯一的标识,只能有唯一的列是主键
 	2. 唯一索引: `unique key`
 	 - 避免重复的列,但索引可以重复即多个列都可以标识为唯一索引
 	3. 常规索引: `key`
 	 - 默认的索引,可以使用index关键字或key关键字声明
 	4. 全文索引: `FulllText`

```mysql
show index from stu -- 显示stu表中使用的索引
alter table `stu` add fulltext index `name`(`name`) -- stu表增加name列为全文索引 `索引名`(`列名`)
create index 索引名 on 表名(`字段名`) -- 创建索引
create 索引类型 index 索引名 on 表名(`字段名`)
explain select * from stu -- 分析sql语句执行的状态
```

**索引原则**

1. 不要对经常变动的数据加索引
2. 小数据量的表不需要索引
3. 索引加在常用来查询的列

# DCL--控制语言

# 权限管理和备份

## 权限

**能够在sqlyog中创建用户并管理权限**

![权限管理](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-05-05_00-16-07.png)

**sql命令**:本质对mysql.user表进行更改

```mysql
create user 用户名 identified by '密码' -- 创建用户并设置密码
set password = PASSWORD('密码') -- 修改当前用户密码
set password for 用户名 = PASSWORD('密码') -- 修改指定用户的密码
rename user 用户名1 to 用户名2 -- 修改用户名
grant all privileges on 库.表 to 用户名 -- 授予用户全部权限除给其他用户授权这一权限外,如果是全部的库和表则为*.*
show grant for 用户名 -- 查看用户权限
show grant for root@localhost -- 查看root权限: grant all privileges on *.* to 'root'@'localhost' with grant option
revoke 权限 on 表.库 from 用户名 -- 撤销用户的权限
drop user 用户名 -- 删除用户
```

## 备份

1. 直接拷贝`data`文件夹,该文件夹就是数据库
2. 在sqlyog中,鼠标移入到数据库或表名上右键备份
3. **在mysql的命令行中导出: `mysql> `**

```mysql
#命令行模式下,导出sql数据库
> mysqldump -h[localhost | 网络主机ip] -uroot -p密码 数据库名 [表1 表2 表3...] >路径
```

```bash
#命令行模式下,导入数据库
mysql> source sql文件路径

#命令行模式下 在stu数据库中导入表
mysql> use `stu`

#导入表
mysql -uroot -p密码 库名< sql文件路径
```

# 设计数据库

## 设计

1. 分析业务和需要处理的数据库需求
2. 概要设计: 设计关系图

## 三大范式

**第一范式（1NF）：要求数据库表的每一列都是不可分割的原子数据项。**

举例说明：

![lizxi](https://images-1300732204.cos.ap-chengdu.myqcloud.com/1218459-20180909201651535-1215699096.png)

在上面的表中，“家庭信息”和“学校信息”列均不满足原子性的要求，故不满足第一范式，调整如下：

![img](https://images-1300732204.cos.ap-chengdu.myqcloud.com/1218459-20180909202243826-1032549277.png)

可见，调整后的每一列都是不可再分的，因此满足第一范式（1NF）；

 

**第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖）**

**第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）。即一个表只描述一件事**

举例说明：

![img](https://images-1300732204.cos.ap-chengdu.myqcloud.com/1218459-20180909204750951-639647799.png)

在上图所示的情况中，同一个订单中可能包含不同的产品，因此主键必须是“订单号”和“产品号”联合组成，

但可以发现，产品数量、产品折扣、产品价格与“订单号”和“产品号”都相关，但是订单金额和订单时间仅与“订单号”相关，与“产品号”无关，

这样就不满足第二范式的要求，调整如下，需分成两个表：

 ![img](https://images-1300732204.cos.ap-chengdu.myqcloud.com/1218459-20180909210444227-1008056975.png)

 

**第三范式（3NF）：在2NF基础上，任何非主[属性](https://baike.baidu.com/item/属性)不依赖于其它非主属性（在2NF基础上消除传递依赖）**

**第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。**

举例说明：

![img](https://images-1300732204.cos.ap-chengdu.myqcloud.com/1218459-20180909211311408-1364899740.png)

上表中，所有属性都完全依赖于学号，所以满足第二范式，但是“班主任性别”和“班主任年龄”直接依赖的是“班主任姓名”，

而不是主键“学号”，所以需做如下调整：

![img](https://images2018.cnblogs.com/blog/1218459/201809/1218459-20180909211539242-1391100354.png) ![img](https://images2018.cnblogs.com/blog/1218459/201809/1218459-20180909211602202-1069383439.png)

这样以来，就满足了第三范式的要求。

ps:如果把上表中的班主任姓名改成班主任教工号可能更确切，更符合实际情况，不过只要能理解就行。

# 数据库驱动和JDBC

**Java操作数据库规范JDBC**

![image-20210506214904907](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210506214904907.png)

1. 添加到lib

   ![image-20210506222811268](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210506222811268.png)

```java
import java.sql.*;

class Main{
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1.加载驱动
       Class.forName("com.mysql.jdbc.Driver"); //现在的版本应该可以不用写该行

        //2.连接数据库,获得数据库对象 honshen是数据库名
        //oracle的数据库链接: url="jdbc:oracle:thin:@localhost:1512:sid"
        String url = "jdbc:mysql://localhost:3306/honshen?useUnicode=true&characterEncodeing=UTF-8&useSSL=true",
         username = "root", //用户名
         password="ys8527"; //密码
        Connection connection = DriverManager.getConnection(url,username,password); //连接数据库,connection为数据库对象

        //3.执行sql对象
        Statement statement = connection.createStatement(); //创建sql语句实例
        //connection.prepareStatement()
        String sql = "select * from usr";   //sql语句
        ResultSet resultSet = statement.executeQuery(sql); //返回结果集对象
        //statement.execute();执行所有sql语句效率较其他低
        //statement.executeBatch();执行多个sql语句
		//statement.executeQuery(); //查询
        //statement.executeUpdate(); //更新插入删除,返回受影响的行数
        
        //4.查看结果集
        //resultSet.next()下一个数据
        while (resultSet.next()){
            System.out.println(resultSet.getObject("usrname")); //查看列名的usrname
        }
        /*指导具体的类型使用具体的类型
        resultSet.getArray();
        resultSet.getInt();
        resultSet.getString();
        ...*/
        
        //关闭连接,依次释放连接
        resultSet.close();
        statement.close();
        connection.close();
    }
}
```

**util工具包**

`db.properties`

```java
dirver = com.mysql,jdbc.Driver
url = "jdbc:mysql://localhost:3306/honshen?useUnicode=true&characterEncodeing=UTF-8&useSSL=true"
username = "root"
password="ys8527"
```

`jdbcUtils`

```java
import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

class jdbcutils {
    private static String driver, url, username, password;

    static {
        try {
            InputStream inputStream = jdbcutils.class.getClassLoader().getResourceAsStream("db.properties");
            Properties properties = new Properties();
            properties.load(inputStream);
            driver = properties.getProperty("driver");
            url = properties.getProperty("url");
            username = properties.getProperty("username");
            password = properties.getProperty("password");
            Class.forName(driver);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, username, password);
    }

    public static void release(Connection connection, Statement statement, ResultSet resultSet) throws SQLException {
        if (resultSet != null)
            resultSet.close();
        if (statement != null)
            statement.close();
        if (connection != null)
            connection.close();
    }
}
```

`Main`

```java
import java.sql.*;

public class Main{
    private static Connection connection;
    private static  Statement statement;
    private static ResultSet resultSet;
    public static void main(String[] args) throws SQLException {
         connection = jdbcutils.getConnection();
         statement = connection.createStatement();
        String sql = "select * from usr";
         resultSet = statement.executeQuery(sql);
        //4.查看结果集
        while (resultSet.next()) {
            System.out.println(resultSet.getObject("usrname"));
        }
        jdbcutils.release(connection,statement,resultSet);
    }
}
```

## 防止sql注入(PreparedStatement)

```java
import java.sql.*;
import java.util.Date;

public class Main{
    private static Connection connection;
    private static  PreparedStatement statement;
    private static ResultSet resultSet;
    public static void main(String[] args) throws SQLException {
         connection = jdbcutils.getConnection();
        String sql = "select * from usr where nurname = ? and pwd = ?"; //?是占位符
        statement = connection.prepareStatement(sql); //预编译sql,不执行
        //手动赋值
        statement.setString(1,"honshen"); //第一个参数
        statement.setString(2,"ys8527"); //第一个参数
        //时间格式要注意,要导入util.Date包
        //statement.setDate(1,new java.sql.Date(new Date().getTime()));

         resultSet = statement.executeQuery(sql);
        //4.查看结果集
        while (resultSet.next()) {
            System.out.println(resultSet.getObject("usrname"));
        }
        jdbcutils.release(connection,statement,resultSet);
    }
}
```

## idea 连接数据库

> 管理员命令行模式下

```mysql
 > mysql -uroot -p
 > #输入密码
mysql> show variables like '%time_zone%';
mysql> set global time_zone = '+8:00'; -- 设置时区为东8区
mysql> flush privileges; -- 使设置生效
```

> 安装idea插件`DB navigator`后重启idea: [https://www.cnblogs.com/kaikai-2018/p/13030719.html](https://www.cnblogs.com/kaikai-2018/p/13030719.html)

# 数据库连接池

**数据库连接释放浪费资源**

DBCP 和C3P0 Druid(阿里巴巴)

---

```java
//依据常用连接数设置一个最小连接数,最大连接数,超过连接则排队等待
//DBCP jar包地址: 
// https://repo1.maven.org/maven2/org/apache/commons/commons-dbcp2/2.8.0/commons-dbcp2-2.8.0.jar
// https://repo1.maven.org/maven2/org/apache/commons/commons-pool2/2.9.0/commons-pool2-2.9.0.jar
```

`dbcp.properties`

```java
driverClassName=com.mysql.jdbc.Driver  // 不多解释，这是基本的驱动加载

url=jdbc:mysql://localhost/db_student    // 驱动注册

username=root    //要连接的数据库用户名

password=root   // 要连接的数据库密码

defaultAutoCommit=true：// 设置是否自动提交,默认为true

defaultReadOnly=false： // 是否为只读 默认为false

defaultTransactionIsolation=3：// 设置数据库的事务隔离级别默认为1，READ_UNCOMMITTED，推荐设置为3

initialSize=10：  // 初始化数据池拥有的连接数量

maxActive=20：  /池中最多可容纳的活着的连接数量，当达到这个数量不在创建连接

maxIdle=20：  // 最大空闲等待，也就是连接等待队列超过这个值会自动回收未使用的连接，直到达到20

minIdle=5： // 最小空闲等待 ,数据池中最少保持的连接

maxWait=10000   // 最大等待时间，超过这个时间等待队列中的连接就会失效

testOnBorrow=true  //从池中取出连接时完成校验 ，验证不通过销毁这个connection，默认为true，

testOnReturn=false  //放入池中时完成校验，默认我fasle

validationQuery=select 1  // 校验语句，必须是查询语句，至少查询一列，设置了它onBorrow才会生效

validationQueryTimeout=1  // 校验查询时长，如果超过，认为校验失败

testWhileIdle=false   // 清除一个连接时是否需要校验

timeBetweenEvictionRunsMillis=1  // DBCP默认有个回收器Eviction，这个为设置他的回收时间周期

numTestsPerEvictionRun=3  // Eviction在运行时一次处理几个连接

poolPreparedStatements=true  //是否缓存PreparedStatements

maxOpenPreparedStatements=1 // 缓存PreparedStatements的最大个数
```

`util/dbcp.java`

```java

import java.io.InputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;
import java.util.Properties;
import org.apache.commons.dbcp2.BasicDataSource;
import org.apache.commons.dbcp2.BasicDataSourceFactory;
 
public class Mydbcp {
    
    // 声明一个DataSource源,也就是驱动
    static BasicDataSource bs = null; 
    // properties集合，读取properties文件
    static Properties properties = new Properties(); 
    
    static {
        
        // 用类加载器加载文件获得流
        InputStream rs = Mydbcp.class.getClassLoader().getResourceAsStream("dbcp.properties");
        try {
            // 加载文件配置内容到集合中
            properties.load(rs);
            // 通过basic工厂获得DataSource源，也就是驱动，相当于注册
            bs = BasicDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
       
        try {
            // 来个for循环测试配置是否正确
            for (int i = 0; i < 50; i++) {
                // 从数据池中取出连接
                Connection connection = bs.getConnection();
//                System.out.println(bs.getMaxTotal());
                // 使用完毕将连接放回数据池(这里是代理过的close方法，并不是JDBC原生的close)
                connection.close();
            }
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        
    }
    
```

```java
//C3P0 jar包地址
// https://repo1.maven.org/maven2/com/mchange/mchange-commons-java/0.2.20/mchange-commons-java-0.2.20.jar
// https://repo1.maven.org/maven2/com/mchange/c3p0/0.9.5.5/c3p0-0.9.5.5.jar
```

`c3p0-config.xml`

```java
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
    <!-- 默认配置，如果没有指定则使用这个配置 -->
    <default-config>
        <property name="user">honshen</property>
        <property name="password">123456</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/zhanghanlun</property>
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="checkoutTimeout">30000</property>
        <property name="idleConnectionTestPeriod">30</property>
        <property name="initialPoolSize">3</property>
        <property name="maxIdleTime">30</property>
        <property name="maxPoolSize">100</property>
        <property name="minPoolSize">2</property>
        <property name="maxStatements">200</property>
    </default-config>
    <!-- 命名的配置,可以通过方法调用实现 -->
    <named-config name="test">
        <property name="user">honshen</property>
        <property name="password">123456</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/zhanghanlun</property>
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <!-- 如果池中数据连接不够时一次增长多少个 -->
        <property name="acquireIncrement">5</property>
        <!-- 初始化数据库连接池时连接的数量 -->
        <property name="initialPoolSize">20</property>
        <!-- 数据库连接池中的最大的数据库连接数 -->
        <property name="maxPoolSize">25</property>
        <!-- 数据库连接池中的最小的数据库连接数 -->
        <property name="minPoolSize">5</property>
    </named-config>
</c3p0-config>
```

`util/c3p0.java`

```java
import com.mchange.v2.c3p0.ComboPooledDataSource;
import java.sql.*;


class jdbcutils {
    public static ComboPooledDataSource dataSource;
    public static void main(String[] args) {
            dataSource = new ComboPooledDataSource("mysql"); //不传参数表示使用默认配置
    }
    public static Connection getConnection() throws SQLException {
        return dataSource.getConnection();
    }

    public static void release(Connection connection, Statement statement, ResultSet resultSet) throws SQLException {
        if (resultSet != null)
            resultSet.close();
        if (statement != null)
            statement.close();
        if (connection != null)
            connection.close();
    }
}
```



