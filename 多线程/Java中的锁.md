本小结参考：[Java 中的锁 - Java 并发性和多线程 - 极客学院Wiki](http://wiki.jikexueyuan.com/project/java-concurrent/locks-in-java.html)

　　锁像 synchronized 同步块一样，是一种线程同步机制，但比 Java 中的 synchronized 同步块更复杂。因为锁（以及其它更高级的线程同步机制）是由 synchronized 同步块的方式实现的，所以我们还不能完全摆脱 synchronized 关键字（译者注：这说的是 Java 5 之前的情况）。

　　自 Java 5 开始，java.util.concurrent.locks 包中包含了一些锁的实现，因此你不用去实现自己的锁了。但是你仍然需要去了解怎样使用这些锁，且了解这些实现背后的理论也是很有用处的。可以参考我对 java.util.concurrent.locks.Lock 的介绍，以了解更多关于锁的信息。

### 一个简单的锁

让我们从 java 中的一个同步块开始：

```java
public class Counter{
    private int count = 0;

    public int inc(){
        synchronized(this){
            return ++count;
        }
    }
}
```

可以看到在 inc()方法中有一个 synchronized(this)代码块。该代码块可以保证在同一时间只有一个线程可以执行 return ++count。虽然在 synchronized 的同步块中的代码可以更加复杂，但是++count 这种简单的操作已经足以表达出线程同步的意思。

以下的 Counter 类用 Lock 代替 synchronized 达到了同样的目的：

```java
public class Counter{
    private Lock lock = new Lock();
    private int count = 0;

    public int inc(){
        lock.lock();
        int newCount = ++count;
        lock.unlock();
        return newCount;
    }
}
```

lock()方法会对 Lock 实例对象进行加锁，因此所有对该对象调用 lock()方法的线程都会被阻塞，直到该 Lock 对象的 unlock()方法被调用。

这里有一个 Lock 类的简单实现：

```java
public class Counter{
public class Lock{
    private boolean isLocked = false;

    public synchronized void lock()
        throws InterruptedException{
        while(isLocked){
            wait();
        }
        isLocked = true;
    }

    public synchronized void unlock(){
        isLocked = false;
        notify();
    }
}
```

注意其中的 while(isLocked) 循环，它又被叫做 “**自旋锁**”。自旋锁以及 wait() 和 notify() 方法在[线程通信](http://wiki.jikexueyuan.com/project/java-concurrent/thread-communication.html)这篇文章中有更加详细的介绍。当 isLocked 为 true 时，调用 lock() 的线程在 wait() 调用上阻塞等待。为防止该线程没有收到 notify() 调用也从 wait() 中返回（也称作[虚假唤醒](http://wiki.jikexueyuan.com/project/java-concurrent/race-conditions-and-critical-sections.html)），这个线程会重新去检查 isLocked 条件以决定当前是否可以安全地继续执行还是需要重新保持等待，而不是认为线程被唤醒了就可以安全地继续执行了。如果 isLocked 为 false，当前线程会退出 while(isLocked) 循环，并将 isLocked 设回 true，让其它正在调用 lock() 方法的线程能够在 Lock 实例上加锁。

当线程完成了[临界区](http://wiki.jikexueyuan.com/project/java-concurrent/race-conditions-and-critical-sections.html)（位于 lock()和 unlock()之间）中的代码，就会调用 unlock()。执行 unlock()会重新将 isLocked 设置为 false，并且通知（唤醒）其中一个（若有的话）在 lock()方法中调用了 wait()函数而处于等待状态的线程。

### 锁的可重入性

Java 中的 synchronized 同步块是可重入的。这意味着如果一个 Java 线程进入了代码中的 synchronized 同步块，并因此获得了该同步块使用的同步对象对应的管程上的锁，那么这个线程可以进入由同一个管程对象所同步的另一个 java 代码块。下面是一个例子：

```java
public class Reentrant{
    public synchronized outer(){
        inner();
    }

    public synchronized inner(){
        //do something
    }
}
```

注意 outer()和 inner()都被声明为 synchronized，这在 Java 中和 synchronized(this) 块等效。如果一个线程调用了 outer()，在 outer()里调用 inner()就没有什么问题，因为这两个方法（代码块）都由同一个管程对象（”this”) 所同步。如果一个线程已经拥有了一个管程对象上的锁，那么它就有权访问被这个管程对象同步的所有代码块。这就是可重入。线程可以进入任何一个它已经拥有的锁所同步着的代码块。

前面给出的锁实现不是可重入的。如果我们像下面这样重写 Reentrant 类，当线程调用 outer() 时，会在 inner()方法的 lock.lock() 处阻塞住。

```java
public class Reentrant2{
    Lock lock = new Lock();

    public outer(){
        lock.lock();
        inner();
        lock.unlock();
    }

    public synchronized inner(){
        lock.lock();
        //do something
        lock.unlock();
    }
}
```

调用 outer() 的线程首先会锁住 Lock 实例，然后继续调用 inner()。inner()方法中该线程将再一次尝试锁住 Lock 实例，结果该动作会失败（也就是说该线程会被阻塞），因为这个 Lock 实例已经在 outer()方法中被锁住了。

两次 lock()之间没有调用 unlock()，第二次调用 lock 就会阻塞，看过 lock() 实现后，会发现原因很明显：

```java
public class Lock{
    boolean isLocked = false;

    public synchronized void lock()
        throws InterruptedException{
        while(isLocked){
            wait();
        }
        isLocked = true;
    }

    ...
}
```

一个线程是否被允许退出 lock()方法是由 while 循环（自旋锁）中的条件决定的。当前的判断条件是只有当 isLocked 为 false 时 lock 操作才被允许，而没有考虑是哪个线程锁住了它。

为了让这个 Lock 类具有可重入性，我们需要对它做一点小的改动：

```java
public class Lock{
    boolean isLocked = false;
    Thread  lockedBy = null;
    int lockedCount = 0;

    public synchronized void lock()
        throws InterruptedException{
        Thread callingThread =
            Thread.currentThread();
        while(isLocked && lockedBy != callingThread){
            wait();
        }
        isLocked = true;
        lockedCount++;
        lockedBy = callingThread;
  }

    public synchronized void unlock(){
        if(Thread.curentThread() ==
            this.lockedBy){
            lockedCount--;

            if(lockedCount == 0){
                isLocked = false;
                notify();
            }
        }
    }

    ...
}
```

注意到现在的 while 循环（自旋锁）也考虑到了已锁住该 Lock 实例的线程。如果当前的锁对象没有被加锁(isLocked = false)，或者当前调用线程已经对该 Lock 实例加了锁，那么 while 循环就不会被执行，调用 lock()的线程就可以退出该方法（译者注：“被允许退出该方法”在当前语义下就是指不会调用 wait()而导致阻塞）。

除此之外，我们需要记录同一个线程重复对一个锁对象加锁的次数。否则，一次 unblock()调用就会解除整个锁，即使当前锁已经被加锁过多次。在 unlock()调用没有达到对应 lock()调用的次数之前，我们不希望锁被解除。

现在这个 Lock 类就是可重入的了。

### 锁的公平性

Java 的 synchronized 块并不保证尝试进入它们的线程的顺序。因此，如果多个线程不断竞争访问相同的 synchronized 同步块，就存在一种风险，其中一个或多个线程永远也得不到访问权 —— 也就是说访问权总是分配给了其它线程。这种情况被称作线程饥饿。为了避免这种问题，锁需要实现公平性。本文所展现的锁在内部是用 synchronized 同步块实现的，因此它们也不保证公平性。[饥饿和公平](http://wiki.jikexueyuan.com/project/java-concurrent/starvation-and-fairness.html)中有更多关于该内容的讨论。



### 在 finally 语句中调用 unlock()

如果用 Lock 来保护临界区，并且临界区有可能会抛出异常，那么在 finally 语句中调用 unlock()就显得非常重要了。这样可以保证这个锁对象可以被解锁以便其它线程能继续对其加锁。以下是一个示例：

```java
lock.lock();
try{
    //do critical section code,
    //which may throw exception
} finally {
    lock.unlock();
}
```

这个简单的结构可以保证当临界区抛出异常时 Lock 对象可以被解锁。如果不是在 finally 语句中调用的 unlock()，当临界区抛出异常时，Lock 对象将永远停留在被锁住的状态，这会导致其它所有在该 Lock 对象上调用 lock()的线程一直阻塞。