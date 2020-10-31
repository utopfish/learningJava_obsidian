## [[MySQL中的索引]]

## 隔离级别
在InnoDB存储引擎中，**Repeatable Read**是默认的事务隔离级别，同时该引擎的实现基于多版本的并发控制协议——**MVCC (Multi-Version Concurrency Control)**，解决了幻读问题，当然 脏读和不可重复读也是不存在的。MVCC最大的好处就在于**读不加锁，读写不冲突**，这样极大的增加了系统的并发性能
