## 简介
[[synchronized与lock的区别]]
[[Synchronized（对象锁）和Static Synchronized（类锁）区别]]
[[volatile 与 synchronized 的区别]]
synchronized 和 Lock 也能够保证可见性，synchronized 和 Lock 能保证同一时刻只有一个线程获取锁然后执行同步代码，并且 在释放锁之前会将对变量的修改刷新到主存当中，因此可以保证可见性
不能由于 synchronized 和 Lock 可以让线程串行执行同步代码，就说它们可以保证指令不会发生重排序
[[synchronized 和可重入锁（ReenTrantLock）的区别]]
[[synchronized与lock的区别]]
[[Synchronized（对象锁）和Static Synchronized（类锁）区别]]
[[CAS与synchronized的使用情景]]
[[volatile 与 synchronized 的区别]]