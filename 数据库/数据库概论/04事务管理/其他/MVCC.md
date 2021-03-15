多版本并发控制（Multi-Version Concurrency Control, MVCC）是 MySQL 的 InnoDB 存储引擎实现隔离级别的一种具体方式，用于实现提交读和可重复读这两种隔离级别。而未提交读隔离级别总是读取最新的数据行，要求很低，无需使用 MVCC。可串行化隔离级别需要对所有读取的行都加锁，单纯使用 MVCC 无法实现。
## 基本思想

在封锁一节中提到，加锁能解决多个事务同时执行时出现的并发一致性问题。在实际场景中读操作往往多于写操作，因此又引入了读写锁来避免不必要的加锁操作，例如读和读没有互斥关系。读写锁中读和写操作仍然是互斥的，而 **MVCC 利用了多版本的思想，写操作更新最新的版本快照，而读操作去读旧版本快照，没有互斥关系**，这一点和 CopyOnWrite 类似。

在 MVCC 中事务的修改操作（DELETE、INSERT、UPDATE）会为数据行新增一个版本快照。

脏读和不可重复读最根本的原因是事务读取到其它事务未提交的修改。在事务进行读取操作时，为了解决脏读和不可重复读问题，MVCC 规定只能读取已经提交的快照。当然一个事务可以读取自身未提交的快照，这不算是脏读。


## [[Undo 日志]]


## [[ReadView]]


## 版本号
MVCC是通过在每行记录后面保存两个隐藏的列来实现的。

1. 数据行的版本号 （DB_TRX_ID）：保存了行的创建时间。
1. 删除版本号 (DB_ROLL_PT)：保存行的过期时间（或删除时间）。

当然存储的并不是实际的时间值，而是系统版本号（system version number)。每开始一个新的事务，系统版本号都会自动递增。事务开始时刻的系统版本号会作为事务的版本号，用来和查询到的每行记录的版本号进行比较。
下面看一下在REPEATABLE READ隔离级别下，MVCC具体是如何操作的。

#### SELECT

InnoDB会根据以下两个条件检查每行记录：
1. InnoDB只查找版本早于当前事务版本的数据行（也就是，行的系统版本号小于或等于事务的系统版本号），这样可以确保事务读取的行，要么是在事务开始前已经存在的，要么是事务自身插入或者修改过的。
2. 行的删除版本要么未定义，要么大于当前事务版本号。这可以确保事务读取到的行，在事务开始之前未被删除。

只有符合上述两个条件的记录，才能返回作为查询结果

#### INSERT
InnoDB为新插入的每一行保存当前系统版本号作为行版本号。
#### DELETE
InnoDB为删除的每一行保存当前系统版本号作为行删除标识。

#### UPDATE
**InnoDB为插入一行新记录，保存当前系统版本号作为行版本号，同时保存当前系统版本号到原来的行作为行删除标识。**

保存这两个额外系统版本号，使大多数读操作都可以不用加锁。这样设计使得读数据操作很简单，性能很好，并且也能保证只会读取到符合标准的行，不足之处是每行记录都需要额外的存储空间，需要做更多的行检查工作，以及一些额外的维护工作

举例说明
```
create table mvcctest( 
id int primary key auto_increment, 
name varchar(20));
```

transaction 1:
```
start transaction;
insert into mvcctest values(NULL,'mi');
insert into mvcctest values(NULL,'kong');
commit;
```
假设系统初始事务ID为1；

|ID 	|NAME |	创建时间 |	过期时间|
|:-----|:-----|:------|:----|
|1 | 	mi |	1 |	undefined
|2 |	kong 	|1 |	undefined

transaction 2:
```
start transaction;
select * from mvcctest;  //(1)
select * from mvcctest;  //(2)
commit
```

#### SELECT

假设当执行事务2的过程中，准备执行语句(2)时，开始执行事务3：

transaction 3:
```
start transaction;
insert into mvcctest values(NULL,'qu');
commit;
```

|ID 	 | NAME |	创建时间 |	过期时间|
|:-----|:-----|:-----|:------|
|1       | 	mi   |	  1 |	undefined |
|2 	    | kong |	1 | 	undefined |
|3 	    | qu     |	 3 |	undefined |

事务3执行完毕，开始执行事务2 语句2，由于事务2只能查询创建时间小于等于2的，所以事务3新增的记录在事务2中是查不出来的，这就通过乐观锁的方式避免了幻读的产生
#### UPDATE

假设当执行事务2的过程中，准备执行语句(2)时，开始执行事务4：

transaction session 4:
```
start transaction;
update mvcctest set name = 'fan' where id = 2;
commit;
```
InnoDB执行UPDATE，实际上是新插入了一行记录，并保存其创建时间为当前事务的ID，同时保存当前事务ID到要UPDATE的行的删除时间

|ID 	| NAME |	创建时间  |	过期时间 |
|:-----|:------|:-------|:-------|
|1  |	mi |	1 |	undefined |
|2 |	kong |	1 |	4 |
|2 	| fan |	4 |	undefined |

事务4执行完毕，开始执行事务2 语句2，由于事务2只能查询创建时间小于等于2的，所以事务修改的记录在事务2中是查不出来的，这样就保证了事务在两次读取时读取到的数据的状态是一致的

#### DELETE
假设当执行事务2的过程中，准备执行语句(2)时，开始执行事务5：

transaction session 5:
```
start transaction;
delete from mvcctest where id = 2;
commit;
```

|ID |	NAME |	创建时间 | 	过期时间 |
|:----|:-----|:----|:-----|
|1 |	mi |	1 |	undefined |
|2 	| kong |	1 |	5 |

事务5执行完毕，开始执行事务2 语句2，由于事务2只能查询创建时间小于等于2、并且过期时间大于等于2，所以id=2的记录在事务2 语句2中，也是可以查出来的,这样就保证了事务在两次读取时读取到的数据的状态是一致的
