

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

在MYSQLD中设置下面的配置文件my.ini/my.cnf

```
log-bin [=path/ [filename]]
expire_logs_days=10   10天更新一次日志
max_binlog_size=100M 单个文件的大小限制
```

关闭重启myslq服务

```
systemctl stop mysqld
systemctl start mysqld
```

查询日志设置

```
show variables like 'log_%';
```

修改日志的名称和路径：my.ini内log-bin修改

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

修改日志文件的存储路径

```
在my.cnf里面修改
log-error=【path】
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
先查看my.cnf中日志的路径，然删除以.err结尾的日志
再mysqladmin -u root -p flush-logs
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

查询：使用vim打开kevin-slow.log文件

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
先在表的某个字段加上索引，然后在用索引查询
expain 查询语句
```

分析表、检查表、优化表

```
analyze table table_name  //分析表，分析数据库的关键字分布
check table table_name   //检查表 ，检查数据库存在的错误
optimize table table_name //优化表，消除“删除”“更新”造成的空间浪费
```

## 优化数据库结构的方法

```
将字段很多的表分解为多个表
给经常用于联合查询的表增加中间表
增加冗余字段
优化插入记录的速度
```



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
见P467
```

```
https://blog.csdn.net/zhangbijun1230/article/details/81608252
```

## 如何使用查询缓冲区

```
用于查询语句较多更新语句较少的情况
my.ini中【mysqld】：
query_cache_size=512M
query_cache_type=1开启缓冲区
```



# 数据库的主从复制

---

![1596781329799](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1596781329799.png)

![1596783741856](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1596783741856.png)

```
主从复制的具体过程：P495
查看slave的复制进度 P505
切换主从服务器： P511
了解服务器的状态：show slave/master staus\G
服务器出错的原因P508
无法启用binlog P515
主从复制不同步的原因 P516
```





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

root密码的修改

```
linux下mysql的root密码忘记解决方法：
1．首先确认服务器出于安全的状态，也就是没有人能够任意地连接MySQL数据库。
因为在重新设置MySQL的root密码的期间，MySQL数据库完全出于没有密码保护的
状态下，其他的用户也可以任意地登录和修改MySQL的信息。可以采用将MySQL对
外的端口封闭，并且停止Apache以及所有的用户进程的方法实现服务器的准安全
状态。最安全的状态是到服务器的Console上面操作，并且拔掉网线。
2．修改MySQL的登录设置：
# vi /etc/my.cnf
在[mysqld]的段中加上一句：skip-grant-tables
例如：
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
skip-grant-tables
保存并且退出vi。
3．重新启动mysqld
# /etc/init.d/mysqld restart
Stopping MySQL: [ OK ]
Starting MySQL: [ OK ]
4．登录并修改MySQL的root密码
# /usr/bin/mysql
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 3 to server version: 3.23.56
Type 'help;' or '\h' for help. Type '\c' to clear the buffer.
mysql> USE mysql ;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed
mysql> UPDATE user SET Password = password ( 'new-password' ) WHERE User = 'root' ;
Query OK, 0 rows affected (0.00 sec)
Rows matched: 2 Changed: 0 Warnings: 0
mysql> flush privileges ;
Query OK, 0 rows affected (0.01 sec)
mysql> quit
Bye
5．将MySQL的登录设置修改回来
# vi /etc/my.cnf
将刚才在[mysqld]的段中加上的skip-grant-tables删除
保存并且退出vi。
6．重新启动mysqld
# /etc/init.d/mysqld restart
Stopping MySQL: [ OK ]
Starting MySQL: [ OK ]
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
mysql -u root -p --html/--xml --execute="select * from dept" test > hhh.html;
mysqldump -u root -p test --default-character-set=utf8 > /newname.sql;
mysql -u root -p --execute="select * from dept;" test > c:\caoxuejie.txt
导入：P417
mysqlimport -u root -p test d:\hhhhh.txt;
mysql>load data infile ‘PATH’ into table 数据库名.表名；
```

![1597376093017](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597376093017.png)

![1597376331433](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597376331433.png)

# 索引



### 索引的优缺点：

![1597371837046](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597371837046.png)



### 索引的创建

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

### 删除索引

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
select a.xxx ,b.xxx from a join b on
a.xxxxx=b.xxxxx;
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

# 表

### 外键

---

先创建有外键的表，再创建另外一个表

![1597111973210](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597111973210.png)

![1597111998229](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597111998229.png)![1597112010643](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597112010643.png)

创建外键：

```
constraint 	外键名 foreign key(dept_no) references dept(d_no);
Constraint 外键约束名 foreign key（外键） references  主表（主键）；创建关联外键约束
```

```
Not null；非空约束
unique；唯一性约束
字段名 数据类型 default 默认值
默认约束；默认为1111；
AUTO_INCREMENT自动递增（必须为主键）
查看表结构 
show create table dept \G;
DESC 表名；
查看表详细结构的命令：
Show create table 表名\G；
```

存储引擎比较

```
InnoDB：提供提交、回滚、事务安全及高并发能力
MYISAM：主要用来插入和查询数据，有着较高的处理数据
```

### 修改表

修改表名

```
alter table table_name rename new_table_name
```

修改表中某一个数据字段的数据类型

```
alter table table_name modify <字段名> <新的数据类型>
```

修改表中某一个字段的字段名

```
alter table table_name change <字段名> <新字段名> <新数据类型>
```

在已存在的表种添加字段

```
alter table table_name add <新字段名> <数据类型>
```

删除字段

```
alter table table_name drop <字段名>
```

修改存储引擎

```
alter table table_name engine=MYISAM
```

删除表的外键约束

```
ALTER table table_name drop foreign key<外键约束名>
```

删除数据表《没有被关联》

```
drop table if exists table_name
```

删除被关联的数据表

```
存在外键时，主表不能直接被删除，必须先解除关联子表的外键约束在删除主表
也就是以上两年条命令合起来就可以
```

![1597127691552](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597127691552.png)

![1597127703136](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597127703136.png)

# 存储过程

---

### 创建存储过程 

```
delimiter //
create procedure 名称(in|out|inout 参数名称 参数类型)
BEGIN
select * from dept；
END //
delimiter ;

参数名称的作用的用于存储结果 ，类型可以时int或者其他，
```

![1597129970250](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597129970250.png)

### 创建存储函数

```
delimiter //
create procedure 函数名称(in|out|inout 参数名称 参数类型)
return type（数据类型）
return（select * from dept；）
//
delimiter ;
```

### 变量的使用

定义一个变量

```
declare var_name [,varname]……date_type [default values]
```

![1597131038485](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1597131038485.png)

变量的赋值

````
set var_name = xxxx;
````

### 存储过程的调用

```
call sp_name([parameter][,....])
```

### 删除存储过程

```
drop procedure proc_name;
drop function func_name;
```

#  视图

创建视图

```
create view view_name(变量) as select 变量 from a where ……；
```

查看视图

```
describe view_name;
```

关于视图的章节：P341

关于触发器的章节 P354

# 触发器

---

# 函数

---

```

数学函数
abs（x）绝对值
pi（）圆周率
sqrt（x）开根
mod（x,y）除余函数
ceil(x)向上取整
floor(x)向下取整
rand()随机函数
round(x)四舍五入
round(x,y)保留小数点后y位
truncate（x,y）截取保留小数点y位
sign（x）返回1、-1、0
pow（x,y）幂运算
exp(x)e的x次方
logn(x)

字符串函数：
char_length(s)长度
length(s) 字符串长度 
concat(s1,s2)合并函数
insert(s1,x,len,s2)插入
lower(s)
Left(s,n)返回从最左边开始n个字符
right(s,n)返回s从最右边开始的n个字符
Lpad（s1，len，s2）从s1左边开始填充由字符串s2填充到len的长度，若s1本身超过了len则直接截断
Rpad（s1，len，s2）从右边开始
Ltrim（s）去除左边的空格
Rtrim（s）去除右边的空格
trim（s）去除所有的空格
repeat（s,n）返回n个相同的s字符串
replace(s,s1,s2) 返回s用s2替代s1
strcmp(s1,s2)相同为0，第一个小于第二个位-1，其余为1
substr(s,n,len)
mid（s，n，len）以上两个的功能相同
locale(str1,str) positon(str1 IN str)  返回str在str1中的位置
ELT（n，‘str1’，str‘2’……）
filed（s，s1，s2……）返回s在列表sn中第一次出现的次数
INSTR(str,str1)  返回str1在str中的位置
reverse(s)反转
find_in_set(s1,s2)返回字串位置的函数

日期和时间函数
curtime()当前时间
localtime()当前时间
now()
unix_timestamp()返回unix时间格式的时间戳
from_unixtime(date) 把unix时间戳转化为普通格式
UTC_DATE()返回世界标准日期
UTC_time()返回世界标准时间
month（date）返回月份的函数
dayname(d)获取星期的函数
weekday()返回日期对应的工作日索引
week（d）一年的第几天
weekofyear（）一年中的第几周
dayofyear（）
dadyofmonth
"/mysql.sh" 72L, 2017C
week(d)指定日期是一年中的第几周
dayofyear(d) 返回日期是一年中的第几天
quarter（d）第几个季度
minute()
second(HH:MM:SS)
addtime(datestamp ,HH:MM:SS)日期相加

time_to_sec("hhmmss")转换成秒
timeformat（time，format）转换时间格式具体的参数看书上176页

--条件判断语句

if（expr ，v1,v2） expr为真则返回v1，否则
ifnull（v1，v2）加入v1不为空则返回v1，否则

--系统信息函数
connection_id（）查看当前用户的连接数
show processlist 输出但钱用户的来连接
user() 、 system_user() 、 current_user() 获取用户名的函数
user（）获取用户名函数
charset（str）返回字符串str的字符集
--加密解密函数
password（str） 、MD5（str）、ENCODE（str，pswd_str）加密函数
DECODE（crypt_str,pswd_str）使用pswd_str解密
inet_aton(‘192.168.1.2’)返回代表ip地址的整数
```

#  考试总结

```
授予用户访问的权限
grant all privileges on *.* to user@host idetified by "passwd";
复制数据：
insert into table1 select * from table
复制表：
create table1 select * from table
```

