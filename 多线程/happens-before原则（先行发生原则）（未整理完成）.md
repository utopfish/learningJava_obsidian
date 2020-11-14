
1. **程序次序规则**：一个线程内，按照代码顺序，书写在前面的操作先行发生于书写在后面的操作。
2. **锁定规则**：一个unLock操作先行发生于后面对同一个锁lock操作。
3. **volatile变量规则**：对一个变量的写操作先行发生于后面对这个变量的读操作。
4. **传递规则**：如果操作A先行发生于操作B，而操作B又先行发生于操作C，则可以得出操作A先行发生于操作C。
5. **线程启动规则**：Thread对象的start()方法先行发生于此线程的每个一个动作。
6. **线程中断规则**：对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生。
7. **线程终结规则**：线程中所有的操作都先行发生于线程的终止检测，我们可以通过Thread.join()方法结束、Thread.isAlive()的返回值手段检测到线程已经终止执行。
8. **对象终结规则**：一个对象的初始化完成先行发生于他的finalize()方法的开始

对于程序次序规则来说，我的理解就是一段程序代码的执行在单个线程中看起来是有序的。注意，虽然这条规则中提到“书写在前面的操作先行发生于书写在后面的操作”，这个应该是程序看起来执行的顺序是按照代码顺序执行的，因为虚拟机可能会对程序代码进行指令重排序。虽然进行重排序，但是最终执行的结果是与程序顺序执行的结果一致的，它只会对不存在数据依赖性的指令进行重排序。因此，在单个线程中，程序执行看起来是有序执行的，这一点要注意理解。事实上，这个规则是用来保证程序在单线程中执行结果的正确性，但无法保证程序在多线程中执行的正确性。

第二条规则也比较容易理解，也就是说无论在单线程中还是多线程中，同一个锁如果出于被锁定的状态，那么必须先对锁进行了释放操作，后面才能继续进行lock操作。

第三条规则是一条比较重要的规则，也是后文将要重点讲述的内容。直观地解释就是，如果一个线程先去写一个变量，然后一个线程去进行读取，那么写入操作肯定会先行发生于读操作。

第四条规则实际上就是体现happens-before原则具备传递性。

什么是happens-before 令A和B表示两组操作，如果A happens-before B，那么由A操作引起的内存变化，在B开始执行之前，都应该是可见的。 A happens-before B，不代表A在B之前执行.
如何确保happen-before 锁（互斥锁、读写锁等）、内存屏障



Happens-before 是用来指定两个操作之间的执行顺序。提供跨线程的内存可见性。

在 Java 内存模型中，如果一个操作执行的结果需要对另一个操作可见，那么这两个操作之间必然存在 happens-before 关系。

上面提到了可以用 volatile 和 synchronized 来保证有序性。除此之外，JVM 还规定了先行发生原则，让一个操作无需控制就能先于另一个操作完成。



主要有以下这些原则：

#### 1. 单一线程原则

> Single Thread rule

在一个线程内，在程序前面的操作先行发生于后面的操作。

<div align="center"><img src="assets/single-thread-rule-1534148720379.png" width=""/></div>



#### 2. 管程锁定规则

> Monitor Lock Rule

对一个锁的解锁（unlock ），总是 happens-before 于随后对这个锁的加锁（lock）

<div align="center"><img src="assets/monitor-lock-rule-1534148737603.png" width=""/></div>



#### 3. volatile 变量规则

> Volatile Variable Rule

对一个 volatile 变量的写操作先行发生于后面对这个变量的读操作。

<div align="center"><img src="assets/volatile-variable-rule-1534148747964.png" width="600"/></div>



#### 4. 线程启动规则

> Thread Start Rule

Thread 对象的 start() 方法调用先行发生于此线程的每一个动作。

<div align="center"><img src="assets/thread-start-rule-1534148760654.png" width="600"/></div>



#### 5. 线程加入规则

> Thread Join Rule

Thread 对象的结束先行发生于 join() 方法返回。

<div align="center"><img src="assets/thread-join-rule-1534148774041.png" width=""/></div>



#### 6. 线程中断规则

> Thread Interruption Rule

对线程 interrupt() 方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过 interrupted() 方法检测到是否有中断发生。



#### 7. 对象终结规则

> Finalizer Rule

一个对象的初始化完成（构造函数执行结束）先行发生于它的 finalize() 方法的开始。



#### 8. 传递性

> Transitivity

如果操作 A 先行发生于操作 B，操作 B 先行发生于操作 C，那么操作 A 先行发生于操作 C。
