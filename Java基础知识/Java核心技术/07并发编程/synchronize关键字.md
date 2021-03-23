## 简介
它修饰的对象有以下几种：
1. 修饰一个代码块，被修饰的代码块称为同步语句块，其作用的范围是大括号{}括起来的代码，作用的对象是调用这个代码块的对象；  
2. 修饰一个**方法**，被修饰的方法称为同步方法，其作用的范围是整个方法，**作用的对象是调用这个方法的对象**；  
3. 修改一个**静态的方法**，其作用的范围是整个静态方法，**作用的对象是这个类的所有对象**；  
4. 修改一个类，其作用的范围是synchronized后面括号括起来的部分，作用主的对象是这个类的所有对象。
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