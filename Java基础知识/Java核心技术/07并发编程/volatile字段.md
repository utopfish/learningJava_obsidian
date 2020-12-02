1. Java 内存模型(JMM)。
2. 重排序与 happens-before 原则了解吗？
3. [[volatile 关键字的作用]]。
4. **禁止进行指令重排序**
5. volatile能保证可见性不能保证原子性：`java.util.concurrent.atomic`包提供原子性操作，对自增，自减，加法，减法进行了封装。`atomic`使用`CAS`实现原子性操作。`CAS`实际上是利用处理器提供的 `CMPXCHG` 指令实现的，而处理器执行 `CMPXCHG` 指令是一个原子性操作。
6. [[volatile 与 synchronized 的区别]]
7. [[volatile 关键字的如何保证内存可见性]]


[[volatile 关键字的作用]]
[[volatile 关键字的如何保证内存可见性]]
[[volatile 与 synchronized 的区别]]
[[volatile对于64位缓存行的优化]]