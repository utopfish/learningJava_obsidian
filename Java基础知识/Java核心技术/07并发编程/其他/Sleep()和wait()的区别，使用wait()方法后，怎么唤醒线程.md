- **sleep方法只让出了CPU，而并不会释放同步资源锁。**
- **wait()方法则是指当前线程让自己暂时退让出同步资源锁**，以便其他正在等待该资源的线程得到该资源进而运行。
- sleep()方法可以在任何地方使用；wait()方法则只能在同步方法或同步块中使用。
- sleep()是线程线程类（Thread）的方法，调用会暂停此线程指定的时间，但监控依然保持，不会释放对象锁，到时间自动恢复；wait()是Object的方法，调用会放弃对象锁，进入等待队列，待**调用notify()/notifyAll()唤醒指定的线程或者所有线程**，才会进入锁池，不再次获得对象锁才会进入运行状态.
- notify让之前调用wait的线程有权利重新参与线程的调度