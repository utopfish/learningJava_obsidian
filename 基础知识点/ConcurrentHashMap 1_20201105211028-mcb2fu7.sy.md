在JDK1.7及其之前ConcurrentHashMap实现线程安全的方法相对比较简单：

- 其内部将数据分为数个“段（Segment）”，其数量和并发级别有关系，具体是“大于等于并发级别的最小的2的幂次”。
- 每个segment使用单独的ReentrantLock（分段锁）。
- 如果操作涉及不同segment，则可以并发执行，如果是同一个segment则会进行锁的竞争和等待。此设计的效率是高于synchronized的。

不过JDK8之后，ConcurrentHashMap舍弃了ReentrantLock，而重新使用了synchronized。其原因大致有一下几点：

- 加入多个分段锁浪费内存空间。生产环境中， map 在放入时竞争同一个锁的概率非常小，分段锁反而会造成更新等操作的长时间等待。
- 为了提高 GC 的效率
