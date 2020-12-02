binlog 是 Mysql sever 层维护的一种二进制日志，与 innodb 引擎中的 redo/undo log 是完全不同的日志；其主要是用来记录对 mysql 数据更新或潜在发生更新的 SQL 语句，并以"事务"的形式保存在磁盘中；

作用主要有：

1. **复制**：MySQL Replication 在 Master 端开启 binlog，Master 把它的二进制日志传递给 slaves 并回放来达到 master-slave 数据一致的目的。
2. **数据恢复**：通过 mysqlbinlog 工具恢复数据。
3. **增量备份**。