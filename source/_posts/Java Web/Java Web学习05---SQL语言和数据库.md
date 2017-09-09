---
layout: post
title: Java Web学习05---SQL语言、数据库和MySQL
comments: true
date: 2016-11-02 16:13:35
tags:
    - Java
    - MySQL
    - 数据库
---
数据库就是存储数据的仓库，其本质是一个文件系统，数据按照特定的格式将数据存储起来，用户可以通过sql语句对数据库中的数据进行增加，修改，删除及查询操作.

<!--more-->

## <font color=orange>SQL语言</font>
SQL(Structured Query Language)语言是1974年由Boyce和Chamberlin提出的一种介于关系代数与关系演算之间的结构化查询语言，是一个通用的、功能极强的关系型数据库语言。
* 书写注意事项
    * SQL语句可以单行或多行书写，以分号结尾
    * MySQL数据库的SQL语句不区分大小写，建议使用大写
    * 语句中值最好使用引号(推荐使用单引号), 如果是int类型可以省略不写.
    * 使用语句时如果出现说明,  可以使用`-- 说明`, 这个不出现在查看建表语句中(中间有空格).
    >   //例如:
    -- 创建用户表(这里是说明)
    create table if not exists User(
        &emsp;&emsp;uid int primary key auto_increment,
        &emsp;&emsp;username varchar(20) not null unique,
        &emsp;&emsp;password varchar(20) not null
    );
* SQL分类, 以后最常用的就是DDL、DML、DQL
    * <font color=orange>数据定义语言</font>：简称DDL(Data Definition Language)，用来定义数据库对象：数据库，表，列等，例如创建、删除、修改数据库和表结构等；
        * 对数据库的操作
            * <font color=orange>创建数据库</font>
                * 基本格式:`create database 数据库名称;` 使用默认字符集
                * 扩展格式:`create database [if not exists] 数据库名称 [character set 字符集] [collate 校对规则];`,     `[]`中的可以省略, 可有可无. 如果使用, 那么需要去掉外面`[]`
            * <font color=orange>删除数据库</font>
                * 格式:`drop database 数据库名称;`
            * <font color=orange>修改数据库</font>
                * 修改数据库编码或者校对规则
                    * 格式:`alter database 数据库名称  character set 编码 collate 校对规则;`
            * 常见的命令
                * <font color=orange>查看所有数据库</font>
                    * 格式:` show databases;`
                * <font color=orange>查看数据库创建语句</font>
                    * 格式:`show create database 数据库名称;`
                * <font color=orange>查看字符集和校对规则</font>
                    * 格式:`show character set;`
        * 对表的操作(需要先选择数据库)
            * <font color=orange>切换数据库</font>
                * 格式:`use 数据库名称;`
            * <font color=orange>查看当前所在的数据库</font>    
                * 格式: `select database();`
            * <font color=orange>查看当前数据库的所有表</font>
                * 格式: `show tables;`
            * <font color=orange>查看表结构</font>
                * 格式: `desc 表名称;`
            * <font color=orange>查看建表语句</font>
                * 格式: `show create table 表名称;`
            * <font color=orange>创建表</font>
                * 格式: `create table 表名(字段描述 ,字段描述 ,....);`
                * 字段描述格式: `字段名称  字段类型  [字段约束]`
            * 修改表
                * <font color=orange>修改表名称---> rename</font>
                    * 格式1: `alter table 表名 rename [to] 新表名;`
                    * 格式2: `rename table 表名 to 新表名;`
                * <font color=orange>添加字段(列)---> add</font>
                    * 格式: `alter table 表名 add [column] 字段描述;` column可以省略, 不省略时去掉外面的`[]` 
                    * 例如: `alter table 表名 add column password varchar(20);`
                * <font color=orange>修改字段(列)的类型---> modify</font>
                    * 格式: `alter table 表名 modify [column] 字段描述;` column可以省略, 不省略时去掉外面的`[]` 
                    * 例如: `alter table 表名 modify column password int(20);`   
                * <font color=orange>修改字段(列)的名称---> change</font>
                    * 格式: `alter table 表名 change [column] 旧字段名称 新字段描述;` column可以省略, 不省略时去掉外面的`[]` 
                    * 例如: `alter table 表名 change column username user_name varchar(20);`   
                * <font color=orange>删除字段(列)---> drop</font>
                    * 格式: `alter table 表名 drop [column] 列名;` column可以省略, 不省略时去掉外面的`[]` 
                    * 例如: `alter table 表名 drop column username;` 
                * <font color=orange>删除表</font>
                    * 格式: `drop table 表名;` 
                    * 例如: `drop table user;` 
            * <font color=orange>给字段、表添加注释, 使用`comment`</font>
                * 创建时添加
                >   create table if not exists User(
                    &emsp;&emsp;uid int primary key auto_increment comment '用户id',
                    &emsp;&emsp;username varchar(20) not null comment '用户名',
                    &emsp;&emsp;password varchar(20) not null comment '密码',
                    &emsp;&emsp;valid int(1) not null default 1 comment '是否可用 0 不可用, 1 可用'
                ) comment = '用户表';
                * 创建后修改注释
                    * 表
                    >   alter table 表名 comment='这是表的注释';  
                    * 字段
                    >   alter table 表名 modify '字段名' datetime default null comment '这是字段的注释' ;
    * <font color=orange>数据操作语言</font>：简称DML(Data Manipulation Language)，用来对数据库中表的记录进行更新，例如：增、删、改表记录；
        * 插入数据
            * <font color=orange>不指定插入列</font>
                * 格式: `insert into 表名 values('字段值1','字段值2',...);`
                * 例如: `insert into user values(null,'laobai','1234','male','laobai@126.com',null,null);`
                * 不指定插入列, 表示插入所有列, 值的个数必须是该表的列个数, 值的顺序必须与表创建时给出的列顺序一致.
            * <font color=orange>插入指定(部分)列值</font>  
                * 格式: `insert into 表名(字段名1,字段名2,...) values('字段值1','字段值2',...);`
                * 例如: `insert into user(username,password,email,registTime) values('laoqi','1234','laoqi@126.com',null);`
                * 字段名是该表中部分字段名
            * <font color=orange>插入全部列值</font>
                * 格式: `insert into 表名(字段名1,字段名2,...) values('字段值1','字段值2',...);`
                * 例如: `insert into user(id,username,password,gender,email,role,registTime)
values(1,'shijin','1234','male','shijin@126.com',null,null);`
                * 字段名必须是该表中所有字段名, 
            * <font color=red>插入数据时需注意: 值必须用引号(建议使用单引号, 如果值是整型数据类型, 引号可以省略).</font>
            * 如果出现:<font color=red> ERROR 1366: Incorrect String value:</font>, 这是因为MySQL编码和命令行(或cmd)编码不至于导致的.
                * 1、运行`show variables like 'character%';`查看所有的mysql编码
                * 2、临时解决方法: `set character_set_client=gbk,character_set_connection=gbk,character_set_results=gbk;`
                    * 重启mysql服务器后恢复
                * 3、Windows永久解决方法: 在安装目录下面修改<font color=orange>`my.ini`</font>, 修改`default-character-set=编码值`后, 重启MySQL即可.
                * 4、在mac上默认是没有配置文件的，需要到`/usr/local/mysql/support-files`目录下将mysql配置文件模板`my-default.cnf`拷贝到`/etc`下，并将文件名改成`my.cnf`. 在`my.cnf`中找到`[client]`和`[mysqld]`分别添加下面两句话. 或者运行`find / -iname "*.cnf" -print`查找.cnf文件
                >   [client]
                default-character-set=utf8
                [mysqld]
                character-set-server=utf8
        * <font color=orange>更新数据</font>
            * 格式: `update 表名 set 字段名1='字段值1', 字段名2='字段值2', ..... [where 条件];`
            * 如果不添加where条件, 那么默认会修改表中所有行的值,  加where之后只会修改符合条件的值.
            * 字段值必须要用引号(推荐单引号), 如果是整型数据可以省略引号
        * <font color=orange>删除数据</font>
            * 格式: `delete from 表名 [where 条件];`
            * 不添加where条件, 会把表中所有数据都删除, 添加了条件只会删除符合条件的数据
        * <font color=orange>几种清空表的方式</font>
            * `drop table 表名;`
                * 会把整个表删除, 删除后表不存在了
            * `delete from 表名;`
                * 属于DML语句, 删除表中数据
                		* MySQL中 delete速度慢
                		* ORACLE中 delete速度快
                * 插入数据口, 如果主键自增, 从最后一条数据的主键+1开始.
                * 可以回滚
                * 不会释放空间
                * ORACLE中可以闪回(flashback)
                * delete会产碎片
                		* ORACLE中可以使用: `alert table 表名 move;`去除碎片
            * `truncate [table] 表名;`
                * 输入DDL语句, truncate是将表结构销毁,再重新创建表结构
                    * MySQL中 truncate速度快
                		* ORACLE中 truncate速度慢(因为undo数据, 还原数据)
                * 插入数据后, 如果主键自增, 从1开始
                * 不能回滚
                * 可以释放空间
                * ORACLE中不能闪回
                * truncate不会产生碎片
    * <font color=orange>数据查询语言</font>：简称DQL(Data Query Language)，用来查询数据库中表的记录。
        * <font color=orange>select基本查询</font>
            * <font color=orange>查询指定列</font>
                * 格式: `select 字段 from 表名;`
            * <font color=orange>查询指定字段信息,如果要查询多个字段</font>
                * 格式: `select 字段1,字段2,...from 表名;	`
            * <font color=orange>查询所有列</font>
                * 格式: `select * from 表名;`
                * 注意: 在实际开发中，不建议使用 
            * <font color=orange>去掉重复记录</font>
                * 格式: `select distinct 字段 from 表名;`
                * distinct的作用是去除重复
            * <font color=orange>使用别名</font>
                * 使用`as '别名'`可以给表中的字段、表设置别名, 别名也可以使用`~`下面的符号括起
                    * 如果别名中没有空格可以省略单引号或者`~`下面的符号括起.
                    * 如果别名中有空格需要别名整体添加`'`或`~`下面的符号括起.
                >   `select name as 书名,price as 价格,category as 类别,pnum as 数量 from products order by pnum desc;`
            * <font color=orange>在查询中可以直接对列进行运算</font>
                * <font color=orange>ifnull函数</font>
                * 在对数值类型的列做运算的时候，如果做运算的列的值为null的时，运算结果都为null，为了解决这个问题可以使用ifnull函数. 
                * 格式:`ifnull(字段,0)`
                * 例如: `select name,ifnull(price,0)+10 from products;` //在查询结果, 价格的基础上+10
            * 连接函数
            	* `select concat('hello', ' world');`
        * <font color=orange>where语句</font>
            * where语句用于条件查询或者修改等
            * 格式: `select 字段  from 表名  where 条件;`或`update 表名 set 字段名 = 'value'  where 条件;`
            * <font color=orange>where条件种类:</font>
                * 1、<font color=orange>比较运算符</font>
                    * `>`、`>=`、`<`、`<=`、`=`、`!=不等于`、`<>不等于`
                * 2、<font color=orange>逻辑运算符</font>
                    * `and`、`or`、`not`
                        * `表达式1 and 表达式2`、`表达式1 or 表达式2`
                        * `not 表达式`
                * 3、<font color=orange>`between value1 and value2`</font> 相当于 `>= and <=	`	
                    * between 后面的值必须是小值 and后面的是大值	
                * 4、<font color=orange>in</font>
                    * 可以比较多个值(也可以是一个值)
                        * `select * from products where price in (10, 20);`
                * 5、<font color=orange>like</font>
                    * 模糊查询
                        * `like 'abc'` 值是 abc
                    * 通配符使用:
                        * 1、`%`: 匹配多个
                            * `like '%abc'` 以abc结尾
                            * `like 'abc%'` 以abc开头
                            * `like '%abc%'` 包含abc
                        * 2、`_`: 匹配个数, 几个`_`即代表几个数, 相当于length操作
                            * `like '_'` 值为1个字符
                            * `like '__'` 值为2个字符
						* like中使用转义字符
							* 使用`escape`定义转义字符: `select * from user where name like '%\_%' escape '\';`
                * 6、<font color=orange>null值操作</font>
                    * `is null;` 判断为空 
                    * `is not null;` 判断不为空 
                * 7、<font color=orange>length</font>
                    * 格式: `length(字段名) = 值`
                    * 计算字段值的长度
                    * 例如: `select * from products where length(name) = 6; ` //name值长度为6
                * 8、<font color=orange>多个字段(包含一个字段)操作</font>
                    * `+`、`-`、`*`、`/`、`%`等
                        * 例如:`select * from products where price*pnum > 10000;`
                * 9、<font color=orange>limit</font>: 对查询结果限制数量, 经常用来做分页
                    * 如果使用一个参数, 表示返回最大的记录数目
                        * 格式: `select * from account where id > 100 limit 100;`检索前100条记录, 相当于 `select * from account where id > 100 limit 0,100;`
                    * 如果给定两个参数，第一个参数指定第一个返回记录行的偏移量，第二个参数指定返回记录行的最大数目。
                        * 格式: `select * from account where id > 100 limit 100,100;`从第101条开始, 检索100条记录
                    * 有的数据库支持: 如果给定两个参数，第二个参数是`-1`, 那么表示从偏移量位置检索到结束
                        * 格式: `select * from account where id > 100 limit 100,-1;`从第101条开始, 检索到结束
            * 查询时间
            	* MySQL中使用: `select now();`
            	* Oracle中使用: `select sysdate from dual;`
            		* 没有时间, 需要使用: `select to_char(sysdate, '格式') from dual;`
            		* 例如: `select to_char(sysdate, 'yyyy-MM-dd HH:mm:ss') from dual;`
        * <font color=orange>order by排序</font>
            * 我们从数据库中查询出的数据经常需要根据某些字段进行排序，可以使用order by 关键字(可以是列名, 表达式, 别名, 序号(从1开始)),后面跟的就是要排序的列,order by 子句是select的最后的一个字句
            * asc 升序 (默认)
            * desc 降序
            * 例如: `select * from products [where 条件] order by pnum desc;` //按照pnum倒叙排序
            * 例如: `select * from products order by pnum asc,price desc;` // 先按pnum升序, 如果pnum相同后再按price倒序.      
        * <font color=orange>聚集函数</font>
            * 查询都是横向查询，它们都是根据条件一行一行的进行判断，而使用聚合函数查询是纵向查询，<font color=red>它是对一列的值进行计算，然后返回一个单一的值；另外聚合函数会忽略空值.</font>
            * 格式: `select 聚集函数 from 表名 [where 条件];`
                * 开发中一般使用: `select count(*) from products;`
            * <font color=orange>count(字段)</font>：统计指定列不为NULL的记录行数
            * <font color=orange>sum(字段)</font>：计算指定列的数值和，如果指定列类型不是数值类型，那么计算结果为0；
            * <font color=orange>max(字段)</font>：计算指定列的最大值，如果指定列是字符串类型，那么使用字符串排序运算；
            * <font color=orange>min(字段)</font>：计算指定列的最小值，如果指定列是字符串类型，那么使用字符串排序运算；
            * <font color=orange>avg(字段)</font>：计算指定列的平均值，如果指定列类型不是数值类型，那么计算结果为0；
                * round(值, 保留几位小数)
                    * 例如:select round(sum(pnum*price)/sum(pnum),2) '平均价格' from products;
        * <font color=orange>group by分组</font>
            * 分组查询是指使用group by字句对查询信息进行分组,例如：我们要统计出products表中所有分类商品的总数量,这时就需要使用group by 来对products表中的商品根据category进行分组操作.
                * 分组后我们在对每一组数据进行统计。
                * 分组操作中的having子句是用于在分组后对数据进行过滤的，作用类似于where条件。
            * having和where的区别
                * 1、having是在分组后对数据进行过滤. where是在分组前对数据进行过滤
                * 2、having后面可以使用分组函数(统计函数),where后面不可以使用分组函数
            * 格式
                * 1、`select 分组字段[,集合函数] from 表名 [where 条件] group by 分组字段;`
                    * 例如: `select category,sum(pnum) from products group by category;`
                * 2、`select 分组字段[,集合函数] from 表名 [where 条件] group by 分组字段 having 条件;`
        * <font color=orange>DQL语句操作总结</font>
            * select，from，where，group by，having，order by；它们的执行顺序是如下：
                * from：首先执行from，找到要查询的表；
                * where：判断条件，筛选符合条件所有记录；
                * group by：根据之前操作对记录按照指定列进行分组
                * having：对分组后的信息进行筛选；
                * select：选择所需要的列信息；
                * order by：对查询信息进行排序。
    * <font color=orange>数据控制语言</font>：简称DCL(Data Control Language)，用来定义数据库的访问权限和安全级别，及创建用户；grant revoke 
* 约束
MySQL中常用的约束有主键约束,非空约束,唯一约束,外键约束.
    * <font color=orange>主键约束(primary key)</font>
        * 用于标识当前记录的字段。可以是一个字段，也可以是多个字段。它的特点是:数据唯一，不能为null.
        * 常用的几种方式
            * 1、定义表，声明字段时，定义主键.特点：primary key 只能修饰一个字段
            >   create table user(
            &emsp;&emsp;id int primary key,//id是主键
            &emsp;&emsp;username varchar(20)
            );
            * 2、定义表，声明字段之后，在约束区域定义主键。特点  [constraint] primary key (字段1,字段2,....) 可以设置多个字段
            >   create table user(
            &emsp;&emsp;id int ,
            &emsp;&emsp;username varchar(20),
            &emsp;&emsp;password varchar(20),
            &emsp;&emsp;primary key(id, username) //id、username是主键
            );
            * 3、定义表，声明字段，表创建之后。修改表结构添加约束。特点：也可以设置多个字段，更灵活。
            >   create table user(
            &emsp;&emsp;id int ,
            &emsp;&emsp;username varchar(20)
            );
            alter table user add primary key(id);//创建表完成后, 设置主键
        * 一般主键id为int的时候,还习惯配合<font color=orange>`auto_increment`</font>使用,使主键自增.插入数据的时候,id列可以不写,写的时候写null,让数据库维护id.
            * 被修饰的字段类型支持自增, int
            * 被修饰的字段必须是一个key,  primary key
        >   create table user(
        &emsp;&emsp;id int primary key auto_increment, //设置自增
        &emsp;&emsp;username varchar(20)
        );
        * 删除主键
            * `alter table 表名 drop primary key;`
    * <font color=orange>唯一约束(unique)</font>
        * 被修饰的字段不能重复,但是对null值不起作用.
        * 当有多个字段作为主键时, 称为联合主键, 都一样才认为是一条记录.
        * 几种常用方式(和主键约束使用方式一样)
            * 1、定义表，声明字段时，定义唯一约束.特点：unique 只能修饰一个字段
            > create table user(
            &emsp;&emsp;id int unique,
            &emsp;&emsp;username varchar(20)
            );
            * 2、定义表，声明字段之后，在约束区域定义唯一约束。特点  [constraint] unique (字段1,字段2,....) 可以设置多个字段
                >   create table user(
                &emsp;&emsp;id int ,
                &emsp;&emsp;username varchar(20),
                &emsp;&emsp;unique(id)
                );
            * 3、定义表，声明字段，表创建之后。修改表结构添加约束。特点：也可以设置多个字段，更灵活。
            >   create table user(
            &emsp;&emsp;id int ,
            &emsp;&emsp;username varchar(20)
            );
            alter table user add unique(id);
    * <font color=orange>非空约束(not null)</font>
        * 被修饰字段不能为null.
        * 使用方式: 定义表,声明字段时，添加非空约束, 还可以使用default,给定一个默认值.
        >   create table user(
        &emsp;&emsp;id int,
        &emsp;&emsp;username varchar(20)  not null default 'xu'
        ); 
    * <font color=orange>外键约束</font>
        * 一般用来描述表与表之间的关系, 在下面的数据库设计中介绍.
* SQL中的类型和对应的Java类型

|Java中的数据类型|MySQL中的数据类型|备注|
|:---:|:---:|:---:|
|byte | tinyint||
|short | smallint||
|int|int(★)||
|long|	bigint||
|float|float||
|double	|double|	double(m,d) m数字长度，d精度及小数位,double(5,2)表示它的最大值是：999.99|
|String	|char 或 varchar()(★MySQL中特有)|	char固定长度的字符串.默认255,如果存储的字符没有达到指定长度，mysql将会在其后面用空格补足到指定长度；varchar可变长度的字符串,长度可以由我们自己指定，它能保存数据长度的最大值是65535，如果存储的字符没有达到指定的长度，不会补足到指定长度；|
|java.sql.Timestamp	|timestamp(★)|	时间戳,格式'YYYY-MM-DD HH:MM:SS'.若设置为空,将该列设置为当前的日期和时间|
||datetime(★)|	时间,日期,格式'YYYY-MM-DD HH:MM:SS'|
|二进制 Blob |	tinyblob 255B  blob  64kb  ongblob  4gb |  |
|长文本 Clob | tinytext 255B  text  64kb   longtext  4gb | |
|java.sql.Date|	date	|日期,格式为yyyy-MM-dd|
|java.sql.Time	|time	|时间,格式为hh:mm:ss|


## <font color=orange>数据库</font>
* 关系数据库
    * 关系数据库(Relationship DataBase Management System 简写:RDBMS) 描述是建立在关系模型基础上的数据库，借助于集合代数等数学概念和方法来处理数据库中的数据。说白了就是描述实体与实体之间的关系的数据库.例如用户购物下订单,订单包含商品.他们之间的关系可以通过E-R图表示.
    * 常见的关系型数据库
        * <font color=orange>Oracle数据库</font>
            * 收费的大型数据库.Oracle公司的产品.Oracle收购SUN公司，收购MYSQL.
        * <font color=orange>SQL Server数据库</font>
            * 微软公司产品 ,收费的中型的数据库.
        * <font color=orange>DB2数据库</font>
            * IBM公司的数据库产品,收费的.银行系统中.
        * <font color=orange>Sybase数据库</font>
            * 已经淡出历史舞台.提供了一个非常专业数据建模的工具<font color=orange>PowerDesigner</font>.
        * <font color=orange>MySQL数据库</font>
            * 开源免费的数据库，小型的数据库.已经被Oracle收购了.MySQL6.x版本也开始收费.
        * <font color=orange>SQLite	</font>
            * 嵌入式的小型数据库,应用在手机端.
            * iOS常用的SQLite三方库: FMDB
    * Java相关的数据库：MySQL，Oracle．
    * .Net、C++一般使用SQL Server.
* 非关系型数据库
    * 存放的是对象如: `redis`、`NO-sql(not only sql)`

## <font color=orange>MySQL</font>
* [社区版下载地址](http://downloads.mysql.com/archives/community/)
* 安装
    * Windows版本
        * 压缩zip版本
            * 1、首先解压压缩文件, <font color=red>注意目录不要出现中文</font>
            * 2、MySQL 5.5.21版本以前需要拷贝`my-huge.ini`、`my-innodb-heavy-4G.ini`、`my-large.ini`、`my-medium.ini`、`my-small.ini`其中的一个命名为<font color=red>`my.ini`</font>, 并且在后面添加
                * my-small.cnf是为了小型数据库而设计的。不应该把这个模型用于含有一些常用项目的数据库。
                * my-medium.cnf是为中等规模的数据库而设计的。如果你正在企业中使用RHEL,可能会比这个操作系统的最小RAM需求(256MB)明显多得多的物理内存。由此可见，如果有那么多RAM内存可以使用，自然可以在同一台机器上运行其它服务。
                * my-large.cnf是为专用于一个SQL数据库的计算机而设计的。由于它可以为该数据库使用多达512MB的内存，所以在这种类型的系统上将需要至少1GB的RAM,以便它能够同时处理操作系统与数据库应用程序。
                * my-huge.cnf是为企业中的数据库而设计的。这样的数据库要求专用服务器和1GB或1GB以上的RAM。
                * 
            >   [mysqld]
            basedir=<font color=red>MySQL路径</font>
            datadir=<font color=red>MySQL路径\\data</font>
        * 安装包
            * 可以选择自定义安装, 手动选择<font color=red>Server data files</font>存放的位置
    * Mac版本
        * .dmg安装包安装
            * 可以在`系统偏好设置`里面控制开和关、可以设置开机启动
            * Mac安装后会自动随机设置一个密码, 如果忘记了可以在Mac的通知栏找到
        * 通过brew安装
            * `brew install mysql`
* 启动MySQL服务器
    * Windows
        * 到MySQL目录下<font color=red>bin</font>目录中, 命令行运行`mysqld --console`
        * 设置了环境变量path, 也可以不进入这个目录运行
    * Mac
        * 安装完成后在<font color=orange>`系统偏好设置`</font>---<font color=orange>`MySQL`</font>---`Start MySQL Server`
* MySql设置成window服务和移除MySQL服务(Windows)
    * 设置
        * 在MySQL目录下的<font color=red>bin</font>目录中, 命令行运行`mysqld --install`
    * 移除
        * 在MySQL目录下的<font color=red>bin</font>目录中, 命令行运行`mysqld --remove`
* 使用(Windows)
    * 登录
        * 首次安装MySQL, 默认用户名是root, 密码为空.
        * 登录命令: mysql [-h主机地址] -u用户名 -p[密码], 如`mysql -uroot –p`或`mysql -uroot`
            * 本地登录可以省略主机地址,  没有密码直接回车即可
    * 启动mysql服务命令 `net start mysql`
    * 关闭mysql服务命令 `net stop mysql`
    * Linux启动mysql: `systemctl start mysqld.service`
    * Linux重启mysql: `systemctl restart mysqld.service`
    * Linux关闭mysql: `systemctl stop mysqld.service`
    * 设置和修改密码
        * Windows
            * 1、停止MySQL服务器: Windows下命令行运行`services.msc`, 找到并停止运行MySQL服务
            * 命令行运行`mysqld   --console  --skip-grant-tables`
            * 新建cmd 运行`update user set password=password('abc') WHERE User='root';`即可把密码修改为`abc`
        * Mac[参考地址](http://www.bubuko.com/infodetail-1907316.html)
            * 01、停止 mysql server.  通常是在 `系统偏好设置` --- `MySQL` --- `Stop MySQL Server`
                * 也可以终端运行`sudo /usr/local/mysql/support-files/mysql.server stop`停止
                * `sudo /usr/local/mysql/support-files/mysql.server start`是开启服务
            * 02、终端运行`sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data &`(如果修改了安装路径, 对应的路径也需要修改)安全模式来重启服务器, 不用输入密码
            * 03、新打开一个终端运行`sudo /usr/local/mysql/bin/mysql -u root`, 以root身份启动MySQL
            * 04、运行`UPDATE mysql.user SET authentication_string=PASSWORD('新密码') WHERE User='root';`, 注意`;`不能少
            * 05、运行`FLUSH PRIVILEGES;`
            * 06、运行`quit;`退出MySQL即可
            * 07、停止MySQL服务`sudo /usr/local/mysql/support-files/mysql.server stop`
            * 08、重新启动MySQL`sudo /usr/local/mysql/support-files/mysql.server start` 
            * 09、再次通过`./mysql -u root -p`使用密码登录, 输入之前输入的新密码
            * 10、运行`show databases; `, 如果出现<font color=red>You must reset your password using ALTER USER statement before executing this statement</font>, 那么必须在设置一次密码
            * 11、运行`SET PASSWORD = PASSWORD('新密码');`
            * 12、再次测试`show databases; `
            * 其他优化,上面的操作，每次都要进入到mysql的目录上进行启动, 可以使用`alias mysql=/usr/local/mysql/bin/mysql`来设置路径
    * 使用mysqladmin命令修改密码(知道密码的情况下)
        * Windows
            * 1、到MySQL目录下<font color=red>bin</font>目录,
            * 命令行运行`mysqladmin -u用户名 -p旧密码 password 新密码`, 如果旧密码没有, 可以省略-p一项可以省略
        * Mac
            * <font color=red>在使用时如果发现`mysql或mysqladmin`命令提示不存在, 是因为系统默认会查找`/usr/bin`下的命令, 如果这个命令不在这个目录下, 会提示`command not found`</font>
                * 推荐使用`alias mysql=/usr/local/mysql/bin/mysql`或者`alias mysqladmin=/usr/local/mysql/bin/mysqladmin`
            * 终端运行`alias mysqladmin=/usr/local/mysql/bin/mysqladmin`
            * 运行`mysqladmin -u root -p password 新密码`, 输入旧的MySQL密码
                * 但是提示`Access denied for user 'root'@'localhost`, 这是因为权限问题, 修改一次密码即可
            * 再次运行上面修改密码的代码即可.
    * <font color=red>备份和还原数据库</font>, 除了可以使用图形化数据库工具以外还可以使用命令来操作
    	* 备份: `mysqldump -u用户名 -p数据库密码 数据库名称>备份路径`
    	* 还原
    		* 方式1: 无需登录数据库
    			* 1、需要手动创建数据库
    			* 2、`mysql -u用户名 -p数据库密码 bak1<备份路径`
    		* 方式2: 需要登录数据库
    			* 1、登录需要还原的数据库
    			* 2、创建并使用数据库
    			* 3、使用`source 备份路径`命令
    * <font color=orange>查询当前数据库编码, 解决插入乱码问题</font>
        * `show variables like "char%"`
        * 临时统一设置: `set names utf8;`
        * 临时逐条设置解决方法: `set character_set_client=gbk,character_set_connection=gbk,character_set_results=gbk;`
            * 临时设置后, 重启mysql服务器后恢复
        * Windows永久解决方法: 在安装目录下面修改<font color=orange>`my.ini`</font>, 修改`default-character-set=编码值`后, 重启MySQL即可.
        * 在Mac上默认是没有配置文件的，需要到`/usr/local/mysql/support-files`目录下将mysql配置文件模板`my-default.cnf`拷贝到`/etc`下，并将文件名改成`my.cnf`. 在`my.cnf`中找到`[client]`和`[mysqld]`分别添加下面两句话
        >   [client]
        default-character-set=utf8
        [mysqld]
        character-set-server=utf8
    * 设置数据库编码
        * `create database test2 character set UTF8;`


    
## <font color=orange>数据库表设计</font>
* 系统设计中，实体之间的关系有三种:一对一，一对多，多对多. 而表与表之间关系是通过外键来维护的. 
    * 一对多
        * 一方, 我们一般称为一表、主表.  
        * 多方, 我们一般称为多表、从表.
        * <font color=orange>外键</font>: 为了表示它们的关系, 我们一般会在多表中增加一个字段, 字段名称自定义(一般使用主表名称_id), 字段类型和主表的主键保持一致, 那么这个字段称为外键, 它可以保证数据的完整性和有效性.
            * 添加外键格式: `alter table 从表名称 add foreign key(外键名称) references 主表名称(主键);`
            * 删除外键格式: `alter table 表1, 表2 drop foreign key 外键名称` 
        * 外键特点
            * 主表中不能删除从表中已引用的数据
            * 从表中不能添加主表中不存在的数据 
            * 外键可以为null
    * 多对多
        * 实际开发中, 一般引入一张中间表, 中间表中存放两张表中的主键, 一般还会将两张表的主键设置为这张中间表的联合主键, 将多对多拆分为两个一对多.
    * 一对一
        * 可以使用外键来约束
        * 可以将两张表合并成一张表
* <font color=orange>多表查询(★)</font>
    * 笛卡尔积: 多张表无条件的联合查询, 没有任何意义
        * 格式: `select 表1.字段名, 表2.字段名 from a, b;` , 其中字段名可以用`*`表示全部字段.
        * 查询的结果是两个表行数的积.
    * <font color=orange>内连接</font>
        * <font color=orange>显式的内连接</font>: `select 表1.字段名, 表2.字段名 from 表1 [inner] join 表2 on 表1和表2的连接条件;` 
        * <font color=orange>隐式的内连接</font>: `select 表1.字段名, 表2.字段名 from 表1, 表2 where 表1和表2的连接条件;`  
    * <font color=orange>外连接</font>
        * <font color=orange>左外连接</font> 
            * 格式:  `select 表1.字段名, 表2.字段名 from 表1 left [outer] join 表2 on 表1和表2的连接条件;`   
            * 先展示join左边的表的所有数据, 然后根据连接条件查询join右边的表, 符合条件的展示, 不符合的以null显示.
        * <font color=orange>右外连接:</font>
            * 格式:  `select 表1.字段名, 表2.字段名 from 表1 right [outer] join 表2 on 表1和表2的连接条件;`  
            * 先展示join右边的表的所有数据, 然后根据连接条件查询join左边的表, 符合条件的展示, 不符合的以null显示.
        * 左、右外连接可以相互转换
    * <font color=orange>子查询</font>
        * 在sql语言中，当一个查询是另一个查询的条件时，称之为子查询.
        * <font color=orange>单行单列子查询</font>
            * 格式: `select * from orders where user_id=(select id from user where username='张三');`
        * <font color=orange>单列多行子查询</font>
            * 可以使用`in`、`any`、`all`操作
                * in (value1, value2, value3): 是查询里面的值.
                * &gt;any：大于子查询中的最小值。
                * &gt;all：  大于子查询中的最大值。
                * <any：小于子查询中的最大值。
                * <all：  小于子查询中的最小值。
                * !=any或<&gt;any：不等于子查询中的任意值。
                * !=all或<&gt;all：不等于子查询中的所有值。
                * =any：等于子查询中任意值。
                >   select * from user where `id` !=all (select DISTINCT `user_id` from orders where `user_id` is NOT NULL);
        * <font color=orange>多行多列子查询</font>
            * 这个子查询就是返回一个表, 然后通过条件关联即可, 子查询的结果作为临时表
            >   select \* from user u ,(select * from orders where price>300) o where o.user_id=u.id;//给表起别名
            * 也可以不使用子查询, 使用内连接
            >   select * from user u,orders o where o.price>300 and o.user_id =u.id; 

## <font color=red>Warning</font>
SQL语言里面的外键约束、unique约束不是一定的, 也可以通过Java程序来控制, 而且添加了外键约束对于开发测试来说很不好.
delete语言也很少使用, 删除一般分为物理删除(delete)和逻辑删除(查询不到, 一般在表中添加一个字段, 来控制是否有效),  一般使用逻辑删除.

## <font color=red> Tips </font>
当有多个输入框, 进行多调价查询的时候, 因为查询的关键字个数不确定, 所以查询条件不好控制, 这个时候可以通过小技巧来写.
* 多个条件同时满足条件
	* 先使用 `where 1 = 1` 条件, 关键字非空再拼接 条件 ` and xxx like(=) ?` 
	* 例如: `select * from account where 1 = 1( and user = admin)( and password = 123456)`
* 多个条件满足一个即可
	* 线使用 `where 1 != 1` 条件, 关键字非空再拼接 条件 ` or xxx like(=) ?`
	* 例如: `select * from account where 1 != 1( or user = admin)( or password = 123456)`