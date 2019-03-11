# MySQL

apache公司   开源共享   免费

mysql [-hlocalhost] -uroot -p   以超级管理员的身份登录

use demo;       查看

- DCL（数据库控制语言）:
- DDL（数据库定义语言）:
- DML（数据库操纵语言）:
- DQL（数据库查询语言）:
- TCL（事务控制语言）：
- 数据库锁：
- 主从配置：


##### 命令行链接方式

```mysql
mysql -u用户名   -p密码  -h服务器IP地址   -P服务器端口MySQL端口号   -D数据库名
mysql -uroot -p9264934.. -hlocalhost -P3306 -Dguanli
```

#### 数据库控制语言命令（DCL）:

##### 创建本地用户

```sql
-- 选择mysql数据库
use mysql;
-- 创建本地用户
create user 'superboy'@'localhost' identified by 'iamsuperboy';
-- 刷新MySQL的系统权限相关表，使添加用户操作生效，以免会出现拒绝访问
flush privileges;
```

##### 创建远程用户

```sql
-- 从192.168.122.12登陆的用户
create user 'superboy'@'192.168.122.12' identified by 'password';
-- 从任意ip登陆的用户
create user 'superboy'@'%' identified by 'password';
-- 不做指定默认为'%'
create user 'superboy' identified by 'password';
```

对用户的基本操作

```sql
 创建用户
 create user 'wang'@'localhost' identified by 'iamsuperboy';
 
 修改用户的权限：
 grant all privileges on *.* to 'wang'@'%';
 
 # all 可以替换为 select,delete,update,create,drop
 -- 赋予部分权限，其中的shopping.*表示对以shopping所有文件操作。
 grant select,delete,update,insert on 数据库.* to 'wang'@'localhost' identified by 
 'superboy';
 
 -- 赋予所有权限
 grant all privileges on 数据库.* to superboy@localhost identified by 'iamsuperboy';
 
 'revoke'
 --撤销权限
 revoke privlieges_type on 权限名 from 用户名
 
 删除用户：
 Delete FROM mysql.user Where user='user_name' and host='localhost' ;
 
 --允许远程链接
 grant all privileges on *.* to 'root'@'%' identified by 'mysql' with grant option;flush privileges;--刷新
```

##### 修改密码

```mysql
1、set password for 'wang'@localhost = '123456'
2、update mysql.user set password=password('新密码') where user='root' and 			      	host='localhost' flush privileges;
3、mysqladmin -u用户名 -p旧密码 password 新密码

#忘记root密码
1、停掉mysql服务  serve mysql stop
2、cmd中进入安装目录bin  然后 mysqld --console --skip-grant-tables --shared-memory  #跳过权限检查
3、再开启另一个cmd进入mysql    #直接输入mysql
4、flush privileges
5、退出，并重启系统
```



##### 数据库备份

```mysql
mysqldump -u [username] -p[password] [database_name] > [文件夹路径]
mysqldump -uroot -p9264934.. -hlocalhost wangwei > mysql


mysqldump -u [username] -p[password] --no-data [database_name] > [文件夹路径] #只要结构，不要数据
mysqldump -uroot -p9264934.. -hlocalhost --no-data wangwei > mysql


mysqldump -u [username] -p[password] --no-create-info [database_name] > [文件夹路径] #只要数据，不要结构
mysqldump -uroot -p9264934.. -hlocalhost --no-create-info wangwei > mysql


mysqldump -u [username] -p[password] [dbname1,dbname2] > [文件路径]  #备份多个数据库
```





##### 数据库查询

- #### 数据库查看

```mysql
- 查看所有的表：show tables

- 查看表的信息： show  full tables;

- 查看相关列：show columns from teach；
			show columns from teach like 'id'；
			show columns from teach like '%e%'； #查看带有 ’e‘的内容
```

- #### 查看用户信息

```mysql
查看当前用户：select user();
			select current_user();
			
查看当前有多少用户登录
			select user,host,db,command from information_schema.procrsslist:
			
```



##### 数据库维护

- ###### 分析表

```mysql
analyze table 表名1 [表名2]
```

- ###### 优化表

```mysql
optimize table 表名
```

- ###### 检查表

```mysql
check table 表名
```

- ###### 修复表

```mysql
repair table 表名
```





- 查看表的字段内容

```py
desc t1;         //自增  primary key//主键
```

- 添加数据

```py
insert into t1 values(1,'tom','man',18);
```

```py
insert into t1 (id,name,sex,age) values(1,'tom','man',18);
```

- sql注入

```javascript
" or 1=1;-- 
```



- 查看数据是否添加成功

```py
select * from t1;
```

- 删除一个表

```py 
drop t1;
```

- 删除表中的一行数据

```
delete from stu where id=1
```

- 替换某字段内容的update语句

```python
update t1 set id=replace(id,1,2);
```

```py
update t1 set sex='woman' where c_id=1;
```



### 数据库定义语言（DDL）

##### 新建数据库

```mysql
create database [if not exists] database_name
```

##### 删除数据库

```mysql
drop database [if exists] database_name
```

##### 创建表

```mysql
create table ceshi1(
id int(10) not null auto_increment primary key,
name varchar(10),
age varchar(10),
sex enum('男','女') NOT NULL,
phone varchar(11) unique,
habbit set("游泳","健身"),
pasword varchar(32))default charset=utf8;    comment='示例';
```



##### 修改表

> alter table 表名   add|drop|modify|change|alter|rename

- alter 增加列

```mysql
alter table user add sex enum("男","女") not null [after name]
```

- alter 增加在table最后一列 的一个属性

```mysql
alter table t2 add hobby char(20);
```

- alter 在table的第一列添加新字段属性

```mysql
alter table t2 add project int first;
```

- alter 在sex字段之后添加新字段num属性

```mysql
alter table t2 add num int after sex;
```

- alter 删除表中的字段

```mysql
alter table t2 drop class_id; //删除class_id字段
```

- alter 添加主键

```mysql
alert table t2 primary key(id)
```

- alter (设置默认值)

```py
ALTER TABLE student ALTER class_id SET DEFAULT 0;//设置默认值为0
ALTER TABLE student ALTER class_id DROP DEFAULT;//删除默认值
```

*为什么有了MODIFY和CHANGE还要来个ALTER呢？这是因为另外两个在修改的时候会把字段之前旧的属性全部覆盖掉

举个例子：现在需要修改class_id DEFAULT的值，我们需要这么写

```
ALTER TABLE student MODIFY class_id VARCHAR(20) DEFAULT 10;
```

而采用ALTER则不需要则可以只需要设置默认值



- ###### modify  &&   change  (修改字段)


> 区别：change必须指定新的字段名而modify则不需要
>
> 如果需要修改字段名只能用CHANGE，否则用哪个都可以

```py
alter table t1 modify num char(10);
//modify 修改num 的数据类型int(10)=>char(10)
```



```py
ALTER TABLE t2 CHANGE class_id c_id VARCHAR(20);
//将class_id字段名改为c_id并修改数据类型为VARCHAR(20)
ALTER TABLE t2 CHANGE class_id class_id VARCHAR(20);
//修改class_id数据类型为VARCHAR(20)不修改字段名
ALTER TABLE t2 MODIFY class_id VARCHAR(20);
//将class_id数据类型修改为VARCHAR(20)
```



- ###### rename (重命名数据表）


```py
alter table t2 rename to t2s;
```

##### `修改存储引擎`

- 修改引擎

```mysql
#先删除再添加
alter table t2s engine=myisam;   //将存储引擎修改为myisam
```

- 删除引擎

```mysql
alter table t2 drop index column_name
```

- 删除引擎

```mysql
alter table t2 engine=InnDB
show engines
show create table 表名
```

- 修改自增值（开始值）

```mysql
alter table t2 auto_increment = 1
```



###### MyISAM

> 不支持事务，不支持外键，访问速度特别快（主要的基本应用为insert，select），创建成功后，有以下三个文件，扩展名分别为（二进制）

- .frm(存储表定义，表结构)
- MYD（MYData存储数据）
- MYI（MYIndex，存储索引）

###### InnoDB

- `健壮的事务型存储引擎`

- `更新密集的表`

- `自动灾难恢复`

- `外键约束`

- `需要事务支持`



###### 索引类型

- 主键索引 primary
  - 一个表中唯一的，在数据的查询，写入，读出能够按照一定的顺序，一定排列进行有序的操作，并且除主键外的其他的字段都会收到其影响
  - 主键的值只能是唯一的，不能重复，auto_increment
- 唯一的键 unique
  - 一个表中能够给多个字段设置唯一的键，他会在查询本字段，形成一定的顺序，分组查询
  - 在本字段中，不能出现相同的内容，除了NULL外
- 普通索引 index
  - 能给多个字段设置普通索引，会在查询本字段
- 文本索引 fulltext   mysql 5.7版本后有效
  - 文本编辑器
  - 帮助我们在大批文本中有序查找内容



###### 外键

- 查看外键

```mysql
show create table student;
```



- 创建外键

```mysql
alter table student
add foreign key(cid)
references classes(cid) on delete cascade;
```

- 删除外键

```mysql
alter table student 
drop key aa;  #删除约束
alter table student 
drop foreign key aa;   #删除键
```




```mysql
1、create table if not exists classes(     #创建主表 ：班级表
cid int(10) auto_increment primary key,
cnaem varchar(20))default charset=utf8


2、create table if not exists student(     #创建副表 ：学生表
sid int(10) auto_increment primary key,
snaem varchar(20),
cid int(10),
constraint aa foreign key(cid) references classes(cid) #  ******加外键
)default charset=utf8
```

- 外键链接下，删除主表内容提示

```mysql
on delete
district（默认）     cascade（同时删除）   no action（什么都不做）   set null(设置为空)

on update
district（默认）     cascade（同时删除）   no action（什么都不做）   set null(设置为空)


create table if not exists student(     #没附表的情况下    创建副表
sid int(10) auto_increment primary key,
snaem varchar(20),
cid int(10),
constraint aa foreign key(cid) references classes(cid) on delete cascade
)default charset=utf8

#或

alter table student     #有副表没外键的情况下
add foreign key(cid)
references classes(cid) on delete cascade;
```



### 数据库操纵语言（DML）

##### insert

- 插入的内容与原unique的id等唯一值冲突的时候

```mysql
insert into student (sid,sname,cid) values (3,ai,3) on duplicate key update sid=sid+1     #student 内有sid=3的数据
```

- 快速复制一个表的结构

```mysql
create table aaa like stu
```

- 快速复制一个表的内容

```mysql
insert into aaa select * from stu
```





- replace

> 可以置换现有的主键或unique

```mysql
replace into aaa (sname, cid) value("zhangsan",3)
```



##### update

update [low_priority][ignore] t1 #low_priority 延迟更新，等没人查询在更新
    set
    	column_name1 = expr1
    	column_name2 = expr2
    	

```mysqle
where
	condition
show full tables
```


-  带有select子句的更新

```mysql
update stu set sname='111' where id=5;
select tname from teach order by id asc;#随机取  desc倒序
select tname from teach order by rand() limit 1;
#两个表之间的更新
update stu set tname=(select tname from teach order by rand() limit 1) where tname is null;
#关联更新
update table1,table2,...
set table1.attr=val,table2.attr=val,...
where condition

update table1 join table2 on...
set table1.attr=val,table2.attr=val,...
where condition
```



```mysql
update classes，student where classes.cname=student.sname set classes.cname="aa",student.sname=""  
```



##### delete

- 带有limit的删除语句

```mysql
delete from student order by id desc limit 1  #删除id从后致前的第一个
```



- 关联删除

```mysql
delete classes,student from classes,student where classes.cname="allj" 
```





##### 清空数据

```mysql
delete fro student     # 逐条删除，主键自增不会从1开始，而是继续，效率低
```

```mysql
truncate [table] student     #自增从1开始  效率高
```



##### 日志管理

> 记录服务器运行信息，通过日志文件可以监视服务器的运行状态和性能，还能对服务器进行排错与故障处理

- MySQL有六种不同类型的日志：

  - 错误日志：记录启动，运行或停止时出现的问题，一般也记录警告信息  一般开启
  - 一般查询日志：记录客户端的链接和执行的语句   一般关闭
  - 慢查询日志：记录所有执行时间超过long_time的所有不适用索引的
  - 二进制日志：数据库信息有任何改变，都会放到二进制日志中 （需要指定）
  - 中继日志：
  - 事务日志：

- 查询变量

  ```mysql
   show global variables [like '%log%']
  ```

- 修改变量

  ```mysql
  set global variables_name=val  
  ```

1. 错误日志

   1. 查看错误日志地址

      ```mysql
      show global variables like  ”log_error"    
      ```

   2. 警告信息开关

      ```mysql
       show global variables like "log_warnings"; #查看是否开启  开启为1，关闭为0
       set global log_warnings=0   #关闭
      ```

2. 一般查询日志

   ```mysql
   1. 启用开关：general_log=(ON|OFF)     #set global general_log=ON   一般时候都关闭
   2. 记录类型：log_output				 #show global variables like 
   3. 查看存储位置：general_log_file       #show global variables like 
   ```

   1. 查看错误日志地址

      ```mysql
      show global variables like  ”log_error"    警告信息开关2.
      ```

      2.错误警告开关

   ```mysql
   show global variables like "log_warnings"; #查看是否开启  开启为1，关闭为0
   set global log_warnings=0   #关闭
   ```

3. 慢查询日志

   > 放到配置文件中

   ```mysql
   查询超时时间： long_query_time=3
   查询慢查询 ： log_slow_querys={YES|NO}
   启动慢查日志： log_query_log=1 (on|off)
   日志记录文件： slow_query_log_file[=file_name]
   ```

### 数据库查询语言（DQL）

> `*`  通配符 所有

| 函数                         | 描述                                   |
| ---------------------------- | -------------------------------------- |
| escape  ‘$’                  | 把 $ 规定为转义字符                    |
| cast(1982-3-1 as data)       | 把1928-3-1转换成data型                 |
| count(cname)                 | cname的条数                            |
| sqlfind_in_set(needle,place) | needle:查询内容        place：所在字段 |

##### select

```mysql
select count(cname) as num from classes
```



```mysql
select

column_1 , column_2,...

from a

	table_1
```

- 模糊查询

  | 内容  |          描述          |
  | :---: | :--------------------: |
  | %老师 |    以老师结尾的数据    |
  | 老师% |    以老师开头的数据    |
  | _老师 | 老师前面只有一个可变量 |
  | 老师_ | 老师后面只有一个可变量 |
  | like  |        精确查询        |
  |   =   |        精确查询        |

- ##### where


- 语句顺序

```mysql
where 

	conditions

group by column_1

having group_conditions

order by column_

limit offset, length
```

```mysql
操作符                                    描述
=                    #等于，几乎任何数据类型都可以用
<> !=                #不等于
<
>
<=
>=

逻辑运算符
or                   #或者
and                  #并且
not					 #非
```

| 操作/逻辑运算符 |  描述  |
| :-------------: | :----: |
|        =        |  等于  |
|      <>!=       | 不等于 |
|        <        |  小于  |
|        >        |  大于  |
|       <=        |        |
|       >=        |        |
|       or        |   或   |
|       not       |   非   |
|       and       |   和   |

- between   包含两端

   (cast(1982-3-1 as data) 把1928-3-1转换成data型)

  ```mysql
  select * from student where birth between cast(1982-3-1 as data) and cast(1988-6-10 as data) ;
  ```

- in

```mysql
select * from classes where sqlfind_in_set(l,cname)   #一个字段中含多个内容
```





- ##### group by


```mysql
select
	c1,c2,c3,....
from
	table
where
	where_conditions
group by t1,t2,t3...;
```



having

> 分组后进行筛选  where是分组前筛选

having后可跟条件（函数）



| 函数    | 描述       |
| ------- | ---------- |
| avg()   | 计算一组值 |
| count() |            |
| instr() |            |
| sum()   |            |
| min()   |            |
| max()   |            |

1、通过时间分类   看一时间段内进货 额

2、通过类别

3、时间、类



- ##### order by


> 对`单列或多列`的`查询结果` 进行`升序或降序`排序

```mysql
select column1,column2,...
from t2
order by num desc，price asc  #降序排列   asc升序    以前一列为基础，再排后一列

select column1,column2,...
from t2
order by field（name,"商品3"，"商品2","商品1"）desc / asc    #自定义排序
```



- ##### limit


> 约束`查询结果`的行数 ，一般跟order by  一起使用

```mysql
select column1,column2,...
from tablename
limit offset , count     #offset：偏移量     count：条数
						 #只有一个参数n：从头开始取n条

select * from goods where cid=1 group by cid desc limit
```





#### 关联查询

> 表与表之间有关系，通过关系去查询 

MySQL支持一下连接：

- 交叉链接

```mysql
select cname,gname
from category cross join goods;

```



- 内链接

```mysql
select
name
from
t1
inner join
t2 on t1.id=t2.id
```



- 左连接

```mysql
select              #以左面为基础
name
from
t1
left join
t2 on t1.id=t2.id
```



- 右连接

```mysql
select              #以右面为基础
name
from
t1
right join
t2 on t1.id=t2.id
```

- 联合查询

```mysql
union    #可以去掉重复项    union all  包括重复项

select cname from category union select gname from goods;
```



#### 子查询

> 把一个查询嵌套在另一个查询，叫内部查询

- 分类
  1. 标量子查询：返回但一直的标量，最简单的形式
  2. 列子查询：返回结果是N行一列
  3. 行子查询：返回结果为一行N列
  4. 表子查询：返回结果为N行N列



1. 标量子查询(一个值)

```mysql
select * from article where uid = (select uid from user where status=1 order by uid desc limit 1)                  in     
```

-   关键字	`any`   `all`

  > any ：< > = <>条件中的任何一个就可以
  > >
  > > all：  <  >  =  <> 条件中的所有条件才可以

  ```mysql
  select * from goods where cid < any (select id from category where id=3 or id=2) 
                                 #all
  ```


   2.列子查询

```mysql

```



#### 函数

##### 聚合函数

>  总值，平均值，最大值，最小值，求和  除count外会忽略null

- count（）

  ```mysql
  select count(*) from category group by cid;
  ```

- avg 平均数

  ```mysql
  
  ```

- num

- max(),min()

- group_conatan()

  > 把分组后的结果连接起来

  ```mysql
  select group_concat(gname) from goods group by cid;   
  ```

- concat() 字符串链接函数

  ```mysql
  select concat("first","last") from names
  
  #concat_wg()   指定连接符号
  select concat_ws("-","first","last") from names
  ```



  - left（） 指定从左至右取的内容长度

  ```mysql
  select left ("abcdef",3)
  "abc"
  ```



  - replace（）  替换，更新

  ```mysql
  select replace（"this is firts","firts","first"）  #把""中的 firts 换成first
  ```



  - substring()  截取  可以从任意位置取

  ```mysql
  select substring("abcdefg"2,2)   #从第二位开始，取两个
  ```



  - trim()   删除不必要的前导或尾随字符

  ```mysql
  select trim({both|leading|trailing} from str)
  
  select trim("    abcd    ")  #默认去掉前后的空格
  select trim(leading from    "abcd"   )  #之去掉前面的
  select trim(leading "a" from    "abcd"   )  #去掉前面的  a
  select trim(both "a" from    "abcd"   )   #去掉所有的  a
  ```





  - format  保留有效数字位数

    > 默认en_US

  ```mysql
  select format(1001.353535,2)   #保留三位
  select format(1001.353535,2，"de_DE")   #默认en_US 改成  de_DE
  ```



##### 时间函数

- curdate()  返回当前的日期

```mysql
select curdate()
```

- now()

```mysql
select now()
select now(),sleep(5),now()
```

- sysdate()

  > 返回指定日期函数

```mysql
select sysdate()
select sysdate(),sleep(5),sysdate()
```

- day()  获取今天几号
- month()

```mysql
select month(now());
```

- year()
- weekday(now()) 获取星期几
- dayname() 获取星期几英文名称

```mysql
set @@lc_time_names = 'zh_CN';     #改中文
select dayname(time) from goods;
```



##### 时间计算函数

- datediff（）

```mysql
select datediff("2017-08-03","2018-08-03")   #相差几天
```



- timediff()

```mysql
select timediff("2017-08-03 12:00:00","2018-08-03 12:00:00")    #相差得时间  
```



- timestampdiff( unit, begin,end)	

  >  unit:microsecond  second  minute  hour  day  week  month  quarter  year

```mysql
#unit:microsecond,second,minute,hour,day,week,month,quarter,year
select timestampdiff(day,"2017-08-03 12:00:00","2018-08-03 12:00:00")    #相差得时间用什																			么单位表示  
```



- date_add(start_date,interval expr unit)

  > 在开始时间上加一个时间后得到的时间

```mysql
select date_add("2018-12-31 23:59:59",interval "1:1" minute_second)
```



#### 视图

> mysql中有一些复杂的语句，对于复杂的查询，每一次查询都是对性能的消耗，而视图就是把第一次所查询出来的内容做成一个表

- 查看所有视图

```mysql
show full view
```

- 查看视图创建过程

```mysql
show create view viewname
```



- 创建视图

```mysql
create view viewname as select ...
```

- 修改视图

```mysel
alter view viewname as select ...
```

- 创建或替换视图

> 没有就创建，有就修改

```mysql
creat or replace view viewname as select ...     #viewname：视图名
```

- 删除视图

```mysql
drop view [databases.name].[viewname]
```



#### 临时表

> 使用频率低，不想每一次都查询，又不想创建视图，临时保存数据，生命周期为数据库使用期间，自动删除

- 创建

```mysql
create temporary table tempname select ...   #没有as   tempname：临时表表名
```



### 事务

> 只有InnoDB支持事务

> 数据库处理操作，执行就好像它是一个单一的一组有序的工作单元，在组内每个单独的操作是成功的，那么一个事务才是完整的，如果事务中任何操作失败，整个事务将失败。

#### 事务性质

- 原子性：确保了工作单位中的所有操作都成功完成：否则，事务被终止，在失败时会被回滚到事务操作以前的状态。
- 一致性：可确保数据库在正确的更改状态在一个成功的结果提交事务。
- 隔离：使事务相互独立地操作。
- 持久性：确保了提交事务的结果或系统故障情况下仍然存在作用。



#### 事务控制语句

- BEGIN或START TRANSACTION;显示地开启一个事务；
- COMMIT;也可以使用COMMIT WORK,不过二者是等价的。COMMIT会提交事务，并使已对数据库进行的所有修改成为永久性的
- ROLLBACK;有可以使用ROLLBACK WORK,不过二者是等价的。回滚会结束用户的事务，并撤销正在进行的所有未提交的修改
- SET AUTOCOMMIT=0 禁止自动提交
- SET AUTOCOMMIT=1 开启自动提交 variable

#### 

### 锁

> 不同的存储引擎支持不同的锁机制 mysql中锁的大分类分为：1、表级锁，2、行级锁，3、页面锁

- 表级锁：开销小，加锁快，不会出现死锁，锁定粒度大，发生锁冲突的概率最高，并发度最低
- 行级锁：开销大，加锁慢，会出现死锁，锁定粒度小，发生锁冲突的概率低，并发度高。



#### 表级锁

> 共享锁：读锁，所有人（包括自己）能查看，不能修改（修改会提示错误），锁期间不能操作别的表
>
> 独占锁：写锁，只有 自己可见可修改，锁期间不能操作别的表

- mysql表级锁存储引擎：
  - MyISAM 引擎
  - MEMORY 引擎

- 加锁（用到MySQL时候，MySQL本身已经给加锁了）

> 演示事务操作的时候    操作多条命令时，不希望被其他用户修改

```mysql
lock tables t1 [t2] read [local],      lock tables t1 [t2] write [local]
```

- 释放锁

```mysql
unlock tables;
```

- 查看表锁征用情况：

  - table_locks_immediate
  - table_locks_waited

  ```mysql
  show status like "table%"  
  show status like "%lock%"
  show processlist   #查看那些当前是哪些命令在等待，从而进行优化
  show open tables   #当前倍速欧珠的表以及锁的次数
  ```





-   ##### 表锁优化


  - optimize table 表名
  - set concurrent_insert=2  允许并发插入
  - 是否设置写的优先级，（登录）
  - 是否设置写内存，解决批量插入数据（新闻系统更新）

- 解决并发问题

  - 并发插入（只能插入，不能修改和删除）

    > MyISAM存储引擎有一个系统变量 concurrent_insert ,专门控制并发插入行为 值可为  0，1，2

    1. cuncurrent_insert 为0时：不允许并发插入
    2. cuncurrent_insert 为1：如果MyISAM没有空洞：即使有锁，也会从尾部插入     有空洞：不插入   （空洞：id=1,2,4  少3）
    3. cuncurrent_insert 为2：加锁的时候把   [local]  加上   allways  总是可以队尾插入信息（锁的情况下）`********很重要，用mysql就改成2`    ` set cuncurrent_insert=2 ` 

- 读写锁的优先级

  - 修改写锁的最大次数

  ```mysql
  set global max_write_lock_count=1      #写一次之后暂停写操作，给读操作机会
  ```

  - 降低写锁的优先级

  ```mysql
  set global low_priority_updates=1
  ```


- 设置写内存

  > 可以根据具体的业务设置读写内存

  ```mysql
  max_allowed_packet=1M      # 限制接受的数据包大小，大的插入和更新会被简直掉
  net_buffer_length=2k       #insert 语句的缓存值 (),(),()  2k-16M
  bulk_insert_buffer_size=8M   #一次性insert语句插入的大小
  ```


#### 间隙锁

> 数据库里id有 1，3，4，5    锁是id>1    再操作id=2的时候也会被锁。所以确定条件的时候一定要有范围

#### 行级锁

> 引擎：InnoDB

> 如果一个事务请求的锁模式与当前的锁兼容，InnoDB就将请求的锁授予该事务；反之如果两者不兼容，该事务就要等待锁释放



-   请求锁是否兼容当前锁模式

|      | X（排他锁） | IX（意向排他锁） | S（共享锁） | IS（意向共享锁） |
| ---- | ----------- | ---------------- | ----------- | ---------------- |
| X    | 冲突        | 冲突             | 冲突        | 冲突             |
| IX   | 冲突        | 兼容             | 冲突        | 兼容             |
| S    | 冲突        | 冲突             | 兼容        | 兼容             |
| IS   | 冲突        | 兼容             | 兼容        | 兼容             |

- 特点

  - 想让InnoDB 上行锁，当前行的操作字段必须要有索引。

  - 如果操作行的操作字段没有索引，行锁会自动升级为表锁。

  - 即使有索引，但用字段的时候修改了类型，索引失效                                                                                       eg:（规定类型：name varchar(255）    命令输入：name=0;）

  - 意向锁是InnoDB自己加的，不用干预；对于update,delete和insert，InnoDB会自动加排他锁（X）；对于select不会自动加共享锁（S）。

  - 当对一行上了排他锁（update，delete，insert），其他用户对这一行数据没有任何权限，但并不影响其他用户修改其他数据。

  - 当对一行上了排他锁（update，delete，insert）后，其他用户可以对当前行进行读操作：修改前的值（InnoDB默认隔离方式）。

  - 研究行锁时，需要将自动提交关闭，

    ```mysql
    set autocommit = 0
    #注：如果有多个客户端，每个都要设置
    ```

- 加锁

  ```mysql
  共享锁：select 后需要加  lock in share mode
  排他锁：select 后需要加  for update
  ```

- 释放锁

  ```mysql
  commit;
  rollback;
  ```

- 查询行锁征用

```mysql
show status like 'innodb_row_lock%';
```

- #### 隔离

- 查看隔离级别

```mysql
select @@session.tx_isolation
```

- 设置隔离级别

```mysql
set session transaction isolation level read uncommitted(读未提交)  #脏读
								       #read committed（不可重复度）
								       #read  （可重复读）          
				#read用到mvcc技术（多版本并发控制）Multiversion Currency control
```

- 隔离级别
  - 可读取未确认（Read uncommitted）

    写事务阻止其他写事务，避免了更新遗失。但是没有阻止其他读事务。

    存在的问题：脏读。即读取到不正确的数据，因为另一个事务可能还没提交最终数据，这个读事务就读取了中途的数据，这个数据可能是不正确的。

    解决办法就是下面的“可读取确认”。

  - 可读取确认（Read committed）

    写事务会阻止其他读写事务。读事务不会阻止其他任何事务。

    大部分数据库使用的默认隔离级别，兼顾速度和正确性。

    存在的问题：不可重复读。即在一次事务之间，进行了两次读取，但是结果不一样，可能第一次id为1的人叫“李三”，第二次读id为1的人就叫了“李四”。因为读取操作不会阻止其他事务。

    解决办法就是下面的“可重复读”。

  - 可重复读（Repeatable read）

    读事务会阻止其他写事务，但是不会阻止其他读事务。

    存在的问题：幻读。可重复读阻止的写事务包括update和delete（只给存在的表加上了锁），但是不包括insert（新行不存在，所以没有办法加锁），所以一个事务第一次读取可能读取到了10条记录，但是第二次可能读取到11条，这就是幻读。

    解决办法就是下面的“串行化”。

  - 可串行化（Serializable）

    读加共享锁，写加排他锁。这样读取事务可以并发，但是读写，写写事务之间都是互斥的，基本上就是一个个执行事务，所以叫串行化。



- 脏读：

  事务A修改了一个数据，但未提交，事务B读到了事务A未提交的更新结果，如果事务A提交失败，事务B读到的就是脏数据。 

- 不可重复读：

  在同一个事务中，对于同一份数据读取到的结果不一致。比如，事务B在事务A提交前读到的结果，和提交后读到的结果可能不同。 

- 幻读：

  在同一个事务中，同一个查询多次返回的结果不一致。通常是因为在事务A中进行了一次全局操作，如新增了一条记录；事务B在事务A提交前后各执行了一次查询操作，发现后一次比前一次多了一条记录。



- 优化行级锁

```mysql
1、精心设计索引，尽量使用索引访问数据
2、选择合理的事务，小事务发生锁冲突的几率小
3、给记录集手动加锁，最好一次性请求足够级别的锁（写锁级别>读锁级别），不要先申请共享再申请排他。
4、尽量用相等的条件访问数据，这样可以避免间隙锁对并发插入的影响
5、对于一些特定事务，可以使用表锁来提高处理速度或减少死锁的可能。（购物，三表同时修改 库存，销售记录，购	物车）
```





- 练习

```text
课程表    id    课程名   老师id

老师表     id   姓名    简介id

简介表     id   信息      

查看 带python老师的信息  3种
```

```mysql
1、select iname from info where id=(select iid from teacher where id=(select tid from class where cid=1));
```

```mysql
2、 SELECT iname FROM info,class,teacher where info.id=teacher.iid and class.tid=teacher.id and class.cid=1;
```

```mysql
3、 select iname FROM info INNER join teacher on info.id=teacher.iid INNER JOIN class on teacher.id=class.tid where class.cid=1;
```

```mysql
4、select iname from info join teacher on info.id=teacher.iid where teacher.id=(select tid from class where cid=1);
```

- explain+语句   测速率 分析语句合理性


### 主从复制

> 实际生产中，有单台MySQL数据库是完全不满足实际需求，无论安全性，高可用性以及高并发性等各方面要求。而主从mysql会实现mysql的实时备份，高可用，读写分离场景。

- 原理



- 配置过程

  - 两台服务器

  - 双方mysql版本一致，如果不一致，从结点高于主节点

  - 服务器防火墙要关闭掉

    `sudo ufw status` `ubuntu系统`

  - 上方数据库用户需要具有远程访问的权限

- 修改主服务器的MySQL的配置文件 window  `放到（my.ini）的mysqld下面` linux(my.cnf)   

  ```mysql
  #mysql 唯一id
  server-id=1
  #二进制文件，此项为必填项，否则不能同步数据 （可以加路径）
  log-bin="mysql-bin"
  #指定二进制错误文件所放的位置
  log-error="mysql-error"
  #需同步的数据库 如果需要同步多个
  binlog-do-db=wangwei
  #binlog-do-db="表名"
  
  #不需要同步的数据库
  binlog-ignore=mysql
  
  read_only   #只读  
  ```

- 给从数据库授权（可以从主服务器取数据）

```mysql
grant replication slave on *.* to 'root'@'192.168.2.155' identified by '9264934.。' flush privileges;
```

- 重启服务器（数据库服务）

```CQL
service mysqld restart   #
```

- 查看主服务器状态

  ```mysql
  show master status;
  ```

- 从服务器配置

  1.修改从服务器的mysql配置文件，注意ID没有被别的mysql服务占用

  ```mysql
  server-id = 2
  log-bin = "mysql-bin"
  replicate-do-db =wangwei
  replicate-ignore-db = mysql 
  ```

  2.重启mysql服务

  3.执行同步sql语句

  ```
  change master to
  master_host='192.168.217.135', #设置要连接的主服务器IP地址master_user='root', #设置要连接的主服务器用户名
  master_password='123456', #设置要连接的主服务器密码
  master_log_file='mysql-bin.000002', #设置要连接的主服务器bin日志的日志名称
  master_log_pos=1041; #设置要连接的主服务器bin日志的记录位置
  
  
  change master to
  master_host = '192.168.217.135',
  master_user = 'root',
  master_password = '123456',
  master_log_file = 'mysql-bin.000008',
  master_log_pos = 107;
  ```

  4.启动slave同步进程

  ```mysql
  start slave
  #查看状态
  show slave status\G   # 查看同步状态
  #其中Slave_IO_Running Slave_SQL_Running值都是YES，表示状态正常
  #如果之前从服务器启动过需要先停止在运行
  stop slave
  ```

- 查看二进制信息

  ```mysql
  show binary logs;   #二进制文件
  show master logs;   #当前使用的
  
  ```

- 删除二进制文件

  ```mysql
  purge binary logs to "mysql-bin.000002";     #删除此文件之前的文件
  ```

  - 自动清理

  ```mysql
  show variable like "expire_logs_day"
  
  set expire_logs_day=7   #7天自动清理一次
  ```

### 优化

- 表级优化（锁）

- 系统优化

  - 主从复制
  - 负载均衡
  - 读写分离


### 

> 多表查询，小表在前，大表在后  （where 小表 .x  =  大表.y）
>
> left join 给左边的表加索引，right 给右边的表加索引

- ### explain

1. #### id:

   - 相同时，由上至下
   - 不同时，从大到小

- 查询语句

  ```tiki wiki
   select * fgroup by
  distanc+-1
  
  ```

- 派生表

  > 在查询内容再查询

  ```mysql
  select aa..cname from (select cname,tid from class) as aa;
  ```

- #### type

  > 从表中找到自己想要的数据的方式
  >
  > ALL<index<range<ref<eq_ref<const<system<NULL

  ```mysql
  ALL:遍历全表找到匹配的行,并且查找的内容不带索引
  index：只遍历索引树，查找索引的列     #加索引
  range: 检索给定的范围，查找的内容不带索引，选择的行带索引   #id<10 尽量不用in  in会破坏掉索引
  ref:确定的一个值    #where name="zhangsan"
  eq_ref:类似ref，区别就在于使用的是唯一索引 unique 
  const:主键关联，主键查询
  system：
  ```

- #### key_len

  > 索引所占的内存空间   utf8  英文占3字节 中文占4字节

- #### Extra

  > 额外信息：

  	1、Using：temporary: 需要额外的内存存储信息
  	
  	2、Impossible whe:条件有问题
  	
  	3、Using filesort:  多次排序，没有索引

  > 复合索引：最佳左前缀；



#### 筛选需优化的内容

	用慢查询日志筛选  自己指定一个时间，超出long_time的语句会存到慢查询的日志中，然后在日志中找到那条语句用explain检测。

#### 优化索引

> 可以把要查找的内容加上索引来补救

- 不能将索引作为表达式的一部分，也不能作为参数，否则索引失效

  ```mysql
  explain select * from class where id+1=1
  explain select tname from teacher where left(tname,3)="";  #没有索引
  ```

- 索引不能做类型转化

  ```mysql
  explain select iid from teacher where tname=111   #tname varchar（）  "1111"
  ```

- 符合索引遵循左前缀策略

- 索引不能跟or 否则全部失效；

- 复合索引不能有不等号 <>  != 或者is null

- 不能用in  可以用between

- 及时删除不用的索引

- like 查询时尽量不要出现左边的“%”

  ```mysql
  explain select iid from teacher where tname like "%xx";
  ```

#### 其他优化

1. 把NULL改成NOT NULL 
2. 根据业务，尽可能选择小的存储数据类型
3. unsigned 表示不允许复制 -127~128   =    0~255
4. timestamp（时间戳） 使用4个字节， 而  datetime  使用8个字节
5. 基本没有使用enum（枚举）
6. 尽量少列 不大于10列
7. 应尽量避免在 where 子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描。 
8. 应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描，如： select id from t where num is null **可以在num上设置默认值0，确保表中num列没有null值**，然后这样查询： select id from t where num=0 
9. 很多时候用 exists 代替 in 是一个好的选择 
10. 用Where子句替换HAVING 子句 因为HAVING 只会在检索出所有记录之后才对结果集进行过滤