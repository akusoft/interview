# MySQL

MySQL 的默认端口为 `3306`，可通过修改 `/etc/my.cnf` 文件更改端口。

MySQL 在 5.1 版本之后，其默认存储引擎为 `InnoDB`，在此之前其默认存储引擎为 `MyISAM`。

## MySQL的事务

事务（Transaction）是数据库并发控制的基本单位，事务可以看作是一系列SQL语句的集合，事务必须要么全部执行成功，要么全部执行失败（回滚）。

事务的四个基本特性（ACID）：

- 原子性（Atomicity）：一个事务中所有操作全部完成或失败
- 一致性（Consistency）：事务开始和结束之后数据完整性没有被破坏
- 隔离性（Isolation）：允许多个事务同时对数据库修改和读写
- 持久性（Durability）：事务结束之后，修改是永久的不会丢失

## MySQL的常用数据类型

- 文本类
  - CHAR
  - VARCHAR
  - TEXT
  - LONGTEXT
- 数值类
  - TINYINT
  - INT
  - BIGINT
  - FLOAT
  - DOUBLE
- 日期类
  - DATETIME
  - TIMESTAMP

## MySQL的索引

索引是**数据表**中一个或者多个列进行排序的数据结构，索引能够大幅提升检索速度，创建、更新索引本身也会耗费空间和时间。

索引类型：

- 普通索引（CREATE INDEX）
- 唯一索引，索引列的值必须唯一（CREATE UNIQUE INDEX）
- 多列索引
- 主键索引，一个表只能有一个
- 全文索引，InnoDB不支持

## MySQL语法

添加和删除数据库：

```SQL
# 显示当前已存在数据库
show databases;

# 创建数据库gc
create database gc;

# 删除数据库gc
drop database gc;

# 使用数据库gc
use gc;
```

添加和删除数据表：

```SQL
# 显示当前已存在数据表
show tables;

# 创建数据表account
create table account
(
id int,
name varchar(255)
);

# 显示数据表account
describe account;

# 删除数据表account
drop table account;
```

添加和删除列：

```SQL
向数据表account中添加整形且不为空默认为1的c1列
alter table account add c1 int(11) not null default 1;

# 删除数据表account中的c1列
alter table account drop c1;
```

修改列或数据表：

```SQL
# 修改数据表account中city列为newcity列，且类型为varchar
alter table account change city newcity varchar(255);

# 修改数据表account的名称为newaccount
alter table account rename newaccount;
```

查看或插入表数据：

```SQL
# 向数据表book中插入一行数据，其中id列为3，title列为't'，content列为'c'
insert into book values(3,'t','c');

# 向数据表book中插入一行数据，其中content列为'c'
insert into book(content) values ('c');

# 将数据表book1中id列不为1的所有数据插入到数据表book2中
insert into book2 select * from book1 where id != 1;

# 从数据表book中查看所有列的数据
select * from book;

# 从数据表book中查看title和content列的数据
select title,content from book;

# 从数据表book中查看id列不为空的所有列的数据
select * from book where id is null;

# 从数据表book中查看title列的数据（去重）
select distinct title from book;

# 从数据表book中查看pages列大于0的数据，结果先按content列降序排列，再按pages列升序排列
select * from book where pages > 0 order by content desc,pages asc;

# 从数据表book中查看所有列的数据，结果按照id列升序排列，并从下标为2的行数据开始，取2行
select * from book order by id limit 2,2;

# 从数据表book中查看title列的数据等于'sun'或'color'的所有列的数据
select * from book where title in ('sun', 'color')；

# 从数据表book中查看1 <= id <= 3的所有列的数据
select * from book where id between 1 and 3;

# 从数据表book中查看id <= 1且id >= 3的所有列的数据
select * from book where id not between 1 and 3;

# 从数据表book中查看title列的数据包含'color'的所有列的数据
select * from book where title like '%color%';

# 从数据表book中查看title列的数据不包含'color'的所有列的数据
select * from book where title not like '%color%';
```

更新表数据：

```SQL
# 修改数据表book中id列为3的数据的content列为'day'
update book set content = 'day' where id = 3;
```
