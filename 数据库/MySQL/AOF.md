## AOF 
AOF 和 RDB 不同， AOF 是通过保存 redis 服务器所执行的写命令来记录数据库状态的。

AOF 通过追加、写入、同步三个步骤来实现持久化机制。

1. 当 AOF 持久化处于激活状态，服务器执行完写命令之后，写命令将会被追加 append 到 aof_buf 缓冲区的末尾。
2. 在服务器每结束一个事件循环之前，将会调用 flushAppendOnlyFile 函数决定是否要将 aof_buf 的内容保存到 AOF 文件中，可以通过配置 appendfsync 来决定。
3. 
```
always ##aof_buf内容写入并同步到AOF文件
everysec ##将aof_buf中内容写入到AOF文件，如果上次同步AOF文件时间距离现在超过1秒，则再次对AOF文件进行同步
no ##将aof_buf内容写入AOF文件，但是并不对AOF文件进行同步，同步时间由操作系统决定
```
如果不设置，默认选项将会是 everysec，因为 always 来说虽然最安全（只会丢失一次事件循环的写命令），但是性能较差，而 everysec 模式只不过会可能丢失1秒钟的数据，而 no 模式的效率和 everysec 相仿，但是会丢失上次同步AOF文件之后的所有写命令数据。
