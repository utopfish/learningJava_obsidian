- 一个是实例锁（锁在某一个实例对象上，如果该类是单例，那么该锁也具有全局锁的概念），一个是全局锁（该锁针对的是类，无论实例多少个对象，那么线程都共享该锁）。
- 实例锁对应的就是 synchronized关 键字，而类锁（全局锁）对应的就是 static synchronized（或者是锁在该类的 class 或者 classloader 对象上）。

```java
/**
 * static synchronized 和synchronized的区别！
 * 关键是区别第四种情况！
 */
public class StaticSynchronized {

    /**
     * synchronized方法
     */
    public synchronized void isSynA(){
        System.out.println("isSynA");
    }
    public synchronized void isSynB(){
        System.out.println("isSynB");
    }

    /**
     * static synchronized方法
     */
    public static synchronized void cSynA(){
        System.out.println("cSynA");
    }
    public static synchronized void cSynB(){
        System.out.println("cSynB");
    }

    public static void main(String[] args) {
        StaticSynchronized x = new StaticSynchronized();
        StaticSynchronized y = new StaticSynchronized();
        /**
         *  x.isSynA()与x.isSynB(); 不能同时访问(同一个对象访问synchronized方法)
         *  x.isSynA()与y.isSynB(); 能同时访问(不同对象访问synchronized方法)
         *  x.cSynA()与y.cSynB(); 不能同时访问(不同对象也不能访问static synchronized方法)
         *  x.isSynA()与y.cSynA(); 能同时访问(static synchronized方法占用的是类锁，
         *                        而访问synchronized方法占用的是对象锁，不存在互斥现象)
         */
    }
}
```
