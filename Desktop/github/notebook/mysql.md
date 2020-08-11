# 数据库日志

日志的查看命令

```
mysql> show global variables like '%log%'; //查看日志信息
mysql> show global variables like 'long%'; //查看慢日志超时时间
mysql> show global variables like "%log_bin%"; //查看二进制日志
```



## 分类

![1596697453831](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1596697453831.png)

## 二进制日志的用法

在MYSQLD中设置下面的配置文件

```
log-bin [=path/ [filename]]
expire_logs_days=10
max_binlog_size=100M
```

关闭重启myslq服务

查询日志设置

```
show variables like 'log_%';
```

查看二进制日志的文件数量和文件名

```
show binary logs;
```

查看二进制文件```

```
mysqlbinlog 日志path
```

删除二进制日志

```
reset master;
```

删除指定日志文件

```
purge {master |binary} logs to 'log_name'//删除比指定编号小的
purge {master |binary} logs before date//删除日期之前的
```

二进制日志恢复数据库

```
mysqlbinlog [option] filename |mysql -uuser -ppass
```

暂停2进制日志

```
set sql_log_bin={0|1}{暂停恢复}
```



## 错误日志的用法

打开错误的日志

```
[mysqld]
log_error=[path /[file_name]]
```

查看错误日志

```
show variable like 'log_error';
```

删除错误日志

```
mysqladmin -u root -p flush-logs;
或者
mysql>flush logs；
```

## 查询通用日志的方法

打开通用日志查询

```
[mysqld]
log
```

删除

```
mysqladmin -u root -p flush-logs
```

执行完成之后发现日志所在目录下重新建立了新的日志文件

## 慢查询日志的方法

开启满慢查询

```
[mysqld]
log-slow-queries[=path / filename]
long_query_time=n//时间值单位是秒
```

![1596763043510](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1596763043510.png)

删除：执行mysqladmin -u root -p flush-logs删除后重新生成日志

或者进入到mysql直接执行命令flush logs；

# 数据库优化

---



![1596765496194](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1596765496194.png)

## 优化查询的方法

查询数据库性能参数

```
show status like 'values';
value:
	connections:
	uptime:mysql上线的时间
	slow_queries慢查询的次数
	com_select查询操作的次数
	com_insert插入操作的次数
	com_update更新操作的次数
	com_delete删除操作的次数
```

使用索引优化查询的方法：

```
expain 查询语句
```

分析表、检查表、优化表

```
analyze table table_name  //分析表
check table table_name   //检查表
optimize table table_name //优化表
```

## 优化数据库结构的方法

![1596765805028](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1596765805028.png)

使用查询缓冲区：

```
[mysqld]
query_cache_size=512M
query_cache_type=1  /表示开启缓冲区
```



## 优化mysql服务器的方法

## 优化服务器硬件

较大的内存

高速磁盘系统

合理分布磁盘IO

配置多处理器

## 优化mysql的参数

```
[mysqld]
key_buffer_size :表示索引缓冲区的大小
table_cache 白哦是同时打开表的个数
query_cache_size: 表示缓冲区的大小
sort_buffer_size:排序缓冲区的大小，排序速度
read_buffer_size: 表示每个线程连续扫描时美俄表分配的缓冲区的大小
详见P467
```

```
https://blog.csdn.net/zhangbijun1230/article/details/81608252
```

# 数据库的主从复制

---

![1596781329799](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1596781329799.png)

![1596783741856](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1596783741856.png)



# 用户

---

查看数据库用户

```
select user,host from mysql.user;
```

添加用户

```
grant all privileges (update、create)  on *.* to user@host identified by 'passwd'
create user caoxuejie@localhost identified by '123';
insert into user (host,user,password) values('localhost','customers',password('customers1'));
```

修改用户的密码

```
set password for caoxuejie@localhost =password('123');
```

普通用户修改密码

```
set password =password("newpasswd");
```

查看用户权限

```
show grants for caoxuejie@localhost
```

删除用户

```
drop user 'user'@'localhost';
```

赋予权限

```
grant all privileges (update、create)  on *.* to user@host identified by 'passwd'
```

回收权限

```
revoke all privileges(update、select) on *.* from caoxuejei@localhost;
```

# 数据备份

---

数据备份

```
数据备份：
mysqldump：
mysqldump -u root -h localhost -p test >/bck.sql; //备份test这个数据库
mysqldump -u root -h localhost -p 数据库名 表名 >/bck.sql; 备份数据库下面的数据表
mysqldump -u root -h host -p --databases 数据库名（多个，空格隔开） >/bck.sql;
mysqldump -u root -h localhost -p --all-databases >/bck.sql; 备份所有数据库
或者直接复制mysql目录：/var/lib/mysql

数据恢复：首先需要创建好原来的数据库名称test
MySQL -u root -p --database test < bck.sql
创建好原来的数据库test，然后进入数据库：
mysql> source /bck.sql
```

相同版本之间的数据库的迁移

```
mysqldump -h www.bac.com -uroot -ppassword dbname |-h www.sdc.com -uroot -ppassword
```

表的导入与导出

```
导出：
mysql -u root -p --html/--xml --execute="select * from dept" test >hhh.html;
mysqldump -u root -p test --default-character-set=utf8 > /newname.sql;
导入：
mysqlimport -u root -p test < hhhhh.txt;
load data infile ‘PATH’ into table 数据库名.表名；
```

# 索引

---

索引的创建

```
create table table_name [col_name data_type] 
[unique、fulltext、spatial] 【index】 【index_name】(col_name [length] [asc |desc])
```

![1597040053158](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597040053158.png)

在已知的表里面添加索引

```
alter table table_name add [unique、fulltext、spatial]index index_name(col_name [length] [asc |desc])

或者
create index index_name on table_name(col);
```

删除索引

```
alter table table_name drop index index_name
或者
drop index index_name on table_name;
```

使用expain语句查看索引的使用情况

```
explain select * from table_name where …… \G
show index from table_name \G;
```

# 插入、更新、删除

---

### 插入数据

```;
insert into table_name (colunm_list) values (values_list)，（values_list）……;
note:字符串类型需要用单引号
	可以单独指定几个特定的字段进行添加
将查询的结果插入的表中：（select加insert就可以）
insert into table_name1 (colunm_list1) select (colunm_list2) from table_name2 where (condition);
```

![1597047249058](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597047249058.png)

### 更新

```
update table_name  set colunm_name1=value1,colunm_name2=value2……where conditions;
note :字符串类型
```

### 删除某条记录

```
delete from teble_name [where <condition>];
```

删除整个表的记录

```
truncate table table_name;
```

# 查询

---

带 in 的查询

```
where id in （111，112）；id号为111和112的记录
```

带 like 的字符匹配查询

```
where name  like cao%
where name like cao_uejie
```

查询空值

```
where id is null/not null
```

or 

```
where id =1 or id =2;
```

limit n 限制前n行

```
select * from dept limit 4
select * from dept limit 4 3 返回第5条开始的后三条
```

内连接

```
select a.xxx ,b.xxx from a join b where a.xxxxx=b.xxxxx;
```

外连接

```
select a.xxx=b.xxx from a left outer join b on a.xxxxx=b.xxx;
左外连接 ：返回左表中所有的字段和右表中连接相等的字段
select a.xxx=b.xxx from a left outer join b on a.xxxxx=b.xxx;
右外连接：返回右表中所有的字段和坐标中相同的字段
```

exists存在性查询

```
select xxx from a where condition and exists (select 语句)；
```

any all

```
select xxx from a where yy>any/all(select语句)
```

union和union all

```
条件：两个表对应的数据类型和列数必须相同
union 可以重复
union all 不可以重复
```

正则表达式

```
select  xxx from a where xxx REGEXP 正则表达式
note：字符串的表示需要加上‘’，就算时有[]也需要‘[]’
note2:'[^abc]'表达式中不含有abc中任意一个
	  '^[abc]'表达式中以abc中任何一个字母开头
```

![1597111151868](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597111151868.png)

# 表-外键

---

先创建有外键的表，再创建另外一个表

![1597111973210](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597111973210.png)

![1597111998229](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597111998229.png)![1597112010643](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597112010643.png)

创建外键：

```
constraint 	外键名 foreign key(dept_no) references dept(d_no);
```

