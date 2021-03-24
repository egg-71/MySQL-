# 1、初识MySQL



javaEE：企业级java开发 web

前端（页面：展示，数据）

后台（连接点：链接数据库jdbc，链接前端（控制，控制视图跳转和给前端传递数据））

数据库（存数据，TXT，Excel，world）

## 1.1为什么学习数据库

1.岗位需求

2.得数据库者得天下

3.需求

4.数据库是所有软件体系中最核心的存在 DBA

## 1.2什么是数据库

数据库（DB，DataBase）

概念：数据仓库，软件，安装在操作系统（window，Linux，mac...）之上，SQL可以存储大量的数据（500万）

作用：存储数据，管理数据

## 1.3数据库分类

**关系型数据库（SQL）**

MySQL Oracle SQL server  DB2 SQLLlite

通过表和表之间，行和列之间的关系进行数据的存储：学员信息表 考勤表



**非关系型数据库（nosql）**

redit mongDB

非关系数据库，对象存储，通过对象的自身属性来决定



**DBMS（数据库管理系统）**

数据库的管理软件，科学有效地管理我们的数据，维护和获取数据；

## 1.4MySQL简介

关系型数据库



安装建议：

1.尽量不要使用exe，不好卸载

2.尽可能用压缩包

## 1.5连接数据库

命令行连接



- ```sql
  mysql -uroot -p     --连接数据库
  --------------------------------------
  update mysql user set authentication_string=password('123456') where user='root' and Host ='localhost';--修改用户密码
  --------------------------------------
  所有的语句都要使用分号“；”结尾
  --------------------------------------
  mysql>use school；--切换数据库
  Database changed
  --------------------------------------
  describe student；--显示数据库中所有的表的信息
  --------------------------------------
  create database westos；--创建一个数据库
  --------------------------------------
  exit；--退出连接
  
  --单行注释（SQL本来的注释）
  /* 多行注释 */ 
  ```

**数据库xxx语言**

DDL    定义

DML   操作

DQL   查询

DCL   控制

# 2、操作数据库

##2.1操作数据库（了解）

操作数据>操作数据库中的表>操作数据库中表的数据

mysql不区分大小写

1.创建数据库

```
CREATE DATABASE [IF NOT EXISTS] westos；
```

2.删除数据库

```
DROP DATABASE [IF EXISTS] westos;
```

3.使用数据库

```
USE `school`
```

4.查看数据库

```
SHOW DATABASE --查看所有的数据库
```

学习思路

- 对照SQLyog可视化历史记录查看SQL
- 固定的语法或关键字必须强行记住

## 2.2数据库的列类型

> 数值

- tinyint	十分小的数据		1字节
- smallint    较小的数据           2字节
- mediumint   中等大小           3字节
- int              标准的整数            4字节
- bigint         较大的数据            8字节
- float            浮点数                   4字节
- double        浮点数                   8字节
- decimal       字符串形式的浮点数（金融计算的时候一般使用）

> 字符串

- char   字符串   0~255
- varchar   可变字符串   0~65535  常用，对应string类型
- tinytext   微型文本   2^8-1
- text   文本串   2^16-1  保存大文本

> 时间日期

- date   YYYY-MM-DD   日期格式
- time   HH：mm：ss   时间格式
- **datetime   YYYY-MM-DD  HH：mm：ss最常用的时间格式**
- **timestamp 时间戳 1970.1.1到现在的毫秒数（全球统一）**
- year   年份表示

> null

- 没有值，位置
- 不要使用null进行运算

## 2.3数据库的字段属性（重点）

**Unsigned**

- 无符号的整数
- 声明了该列不能声明为负数

**zerofill**

- 0填充
- 不足的位数用0填充

**自增**

- 自动在上一条记录的基础上+1（默认）
- 通常用来设计唯一的主键-index，必须是整数类型
- 可以自定义设计主键自增的起始值和步长

**非空**

- NULL not null
- 假设设置为not null，如果不给它赋值，就会报错
- NULL，如果不填写值，默认就是null

**默认**

- 设置默认的值
- sex，默认值为男，如果不指定该列的值，则会有默认的值

## 2.4创建数据库表

```
CREATE TABLE IF NOT EXISTS `student`(
	`id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
	`name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
	`pwd` VARCHAR(20) NOT NULL DEFAULT '女' COMMENT 'sex',
	`birthday` DATETIME DEFAULT NULL COMMENT 'birthday',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
	`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8
```

常用命令

~~~
SHOW CREATE DATABASE school  --查看创建数据库的语句

SHOW CREATE TABLE student --查看student数据表的定义语句

DESC student --显示表的结构

~~~

## 2.5数据表的类型

~~~
--关于数据库引擎
/*
INNODB 默认使用（事务型数据库）
MYISAM 早些年使用（仓库型数据库）
*/
~~~

|              | MYISAM | INNODB                 |
| ------------ | ------ | ---------------------- |
| 事物支持     | 不支持 | 支持                   |
| 数据行锁定   | 不支持 | 支持                   |
| 外键约束     | 不支持 | 支持                   |
| 全文索引     | 支持   | 不支持                 |
| 表空间的大小 | 较小   | 较大，约为MYISAM的两倍 |

常规使用操作：

- MYISAM体积小，运行速度快
- INNODB安全性高，多表多用户操作

## 2.6修改删除表

> 修改

~~~
--修改表名
ALTER TABLE student1 RENAME AS student
--增加表的字段
ALTER TABLE student ADD ages INT (11)
--修改表的字段
ALTER TABLE student MODIFY ages VARCHAR(11)
~~~



> 删除

```
--删除表（如果表存在再删除）
DROP TABLE IF EXISTS teacher
```

**所有的创建和删除操作尽量加上判断以免报错**

# 3、MySQL的数据管理

## 3.1外键（了解）

> 方式一  在创建表的时候增加约束（较麻烦且复杂）

~~~
CREATE TABLE `grade`(
	`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT `年纪id`,
	`gradename` VARCHAR(50) NOT NULL COMMENT `年纪名称`,
	PRIMARY KEY (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8
--学生表的gradeid字段，要去引用年级表的gradeid
--定义外键key
--给这个外键添加约束
CREATE TABLE IF NOT EXISTS `student`
(
	`id` INT(4) NOT NULL AUTO_INCEREMENT COMMENT `学号`,
	`name` VARCHAR(30) NOT NULL DEFAULT `匿名` COMMENT `姓名`,
	`pwd` VARCHAR(20) NOT NULL DEFAULT `123456` COMMENT `密码`,
	`sex` VARCHAR(2) NOT NULL DEFAULT `女` COMMENT `性别`,
	`birthday` DATETIME DEFAULT NULL COMMENT `出生日期`,
	`gradeid` INT(10) NOT NULL COMMENT '学生的年级',
	PRIMARY KEY(`id`),
	KEY `FK_gradeid`(`gradeid`),
	CONSTRAINT `FK_gradeid` FOREIGN KEY(`gradeid`) REFERENCES grade(`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8
~~~

> 方式二 创建表成功后，添加外键约束

~~~
CREATE TABLE `grade`(
	`gradeid` INT(10) NOT NULL AUTO_INCEREMENT COMMENT `年级id`,
	`gradename` VARCHAR(50) NOT NULL COMMENT `年级名称`,
	PRIMARY KEY (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8
--学生表的gradeid字段要引用年级表的gradeid
--定义外键key
--给这个外键添加约束（执行引用）references引用
CREATE TABLE IF NOT EXISTS `student`
(
	`id` INT(4) NOT NULL AUTO_INCEREMENT COMMENT `学号`,
	`name` VARCHAR(30) NOT NULL DEFAULT `匿名` COMMENT `姓名`,
	`pwd` VARCHAR(20) NOT NULL DEFAULT `123456` COMMENT `密码`,
	`sex` VARCHAR(2) NOT NULL DEFAULT `女` COMMENT `性别`,
	`birthday` DATETIME DEFAULT NULL COMMENT `出生日期`,
	`gradeid` INT(10) NOT NULL COMMENT '学生的年级',
	PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8
--创建表的时候没有外键关系
ALTER TABLE `student`
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY(`gradeid`) REFERENCES `grade` (`gradeid`);
~~~

以上操作都是物理外键，数据库级别的外键，不建议使用（避免数据库过多造成困扰   **了解即可**）

最佳实践：

- 数据库就是单纯的表，用来存数据，只有行（数据）和列（字段）

- 如果要使用多张表，使用外键用程序去实现

- 外键必须在应用层完成

##3.2DML语言（重点）

**数据库意义：数据存储、数据管理**

- insert
- update
- 

## 3.3添加

> insert

--插入语句（添加）**一般写插入，数据与字段一定要一一对应**

--insert into 表名（）values()

例：insert into ‘grade’ values('大三')

--插入多个字段

insert into 'grade'('gradename') values('大二'),('大一')

insert into 'student' ('name') values ('张三')

insert into 'student' ('name','pwd','sex') values ('张三','aaa','男')

**语法：insert into 表名（[字段名1，字段2，字段3]）values （'值1'），（'值2'），（'值3'）**

**注意事项**

​	1.字段和字段之间使用英文逗号隔开

​	2.字段是可以省略的，但是后面的值必须要一一对应

​	3.可以同时插入多条数据，values后面的值需要使用逗号隔开（），（），（）

## 3.4修改（update）

//修改名字

update 'student' set 'name'='楚楚' where id=1;（指定条件）

update 'student' set 'name'='王者荣耀'(不指定条件会改动所有的表，不建议使用)

--修改多个属性用逗号隔开

update 'student' set 'name'='楚楚'，email='1084900624@qq.com' where id=1;

----语法

update 表名 set column_name=value where[条件]

| 操作符          | 含义                   | 范围      | 结果  |
| --------------- | ---------------------- | --------- | ----- |
| =               | 等于                   | 5=6       | false |
| <>或!=          | 不等于                 | 5<>6      | true  |
| >               |                        |           |       |
| <               |                        |           |       |
| <=              |                        |           |       |
| >=              |                        |           |       |
| between...and.. | 在某个范围内，闭合区间 |           |       |
| and             | 和                     | 5>1and4>3 | true  |
| or              | 或                     |           |       |

**语法：**

update 表名 set column_name=value where[条件]

**注意：**

- column_name 是数据库的列，尽量带上``
- 条件，筛选的条件，如果没有指定，则会修改所有列
- value，是一个具体的值
- 多个属性之间用逗号隔开

## 3.5删除（delete）

