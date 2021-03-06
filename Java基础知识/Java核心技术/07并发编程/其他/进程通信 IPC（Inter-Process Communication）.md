## **管道（Pipe）** 
管道可用于具有亲缘关系进程间的通信，允许一个进程和另一个与它有共同祖先的进程之间进行通信。**速度慢，容量有限。**
限制：
- 只支持半双工通信（单向交替传输）；
- 只能在父子进程或者兄弟进程中使用。
![[管道通信.bmp]]
## **命名管道（named pipe）** 
也叫FIFO，命名管道克服了**管道没有名字**和**只能在父子进程中使用**的限制。命名管道在文件系统中有对应的文件名。命名管道通过命令mkfifo或系统调用mkfifo来创建。
```c
#include <sys/stat.h>
int mkfifo(const char *path, mode_t mode);
int mkfifoat(int fd, const char *path, mode_t mode);
```
FIFO 常用于客户-服务器应用程序中，FIFO 用作**汇聚点**，在客户进程和服务器进程之间传递数据。
![[命名管道作为汇聚点.bmp]]
## **信号（Signal）**
信号是比较复杂的通信方式，用于通知接受进程有某种事件发生，除了用于进程间通信外，进程还可以发送 信号给进程本身；Linux除了支持Unix早期信号语义函数sigal外，还支持语义符合Posix.1标准的信号函数sigaction（实际上，该函数是基于BSD的，BSD为了实现可靠信号机制，又能够统一对外接口，用sigaction函数重新实现了signal函数）。
## **[[消息队列（Message）]]**
#### 为什么用？
当系统中出现“生产“和“消费“的速度或稳定性等因素不一致的时候，就需要消息队列。

**相比于 FIFO，消息队列具有以下优点**：
- 消息队列可以独立于读写进程存在，从而避免了 FIFO 中同步管道的打开和关闭时可能产生的困难。
- 避免了 FIFO 的同步阻塞问题，不需要进程自己提供同步方法。
- 读进程可以根据消息类型有选择地接收消息，而不像 FIFO 那样只能默认地接收。
#### 好处
1. **提高系统响应速度**
使用了消息队列，生产者一方，把消息往队列里一扔，就可以立马返回，响应用户了。无需等待处理结果。
>处理结果可以让用户稍后自己来取，如医院取化验单。也可以让生产者订阅（如：留下手机号码或让生产者实现listener接口、加入监听队列），有结果了通知。获得约定将结果放在某处，无需通知。
1. **提高系统稳定性**
考虑电商系统下订单，发送数据给生产系统的情况。电商系统和生产系统之间的网络有可能掉线，生产系统可能会因维护等原因暂停服务。如果不使用消息队列，电商系统数据发布出去，顾客无法下单，影响业务开展。两个系统间不应该如此紧密耦合。应该通过消息队列解耦。同时让系统更健壮、稳定。


## **共享内存** 
使得多个进程可以访问同一块内存空间，是**最快**的可用 **IPC** 形式。是针对其他通信机制运行效率较低而设计的。往往与其它通信机制，如信号量结合使用，来达到进程间的同步及互斥。**能够很容易控制容量，速度快，但要保持同步，比如一个进程在写的时候。**
## **内存映射（mapped memory）** 
内存映射允许任何多个进程间通信，每一个使用该机制的进程通过把一个共享的文件映射到自己的进程地址空间来实现它。
## **信号量（semaphore）** 
主要作为进程间以及同一进程不同线程之间的同步手段。**不能传递复杂消息，只能用来同步。**
## **套接口（Socket）** 
更为一般的进程间通信机制，可用于不同机器之间的进程间通信。起初是由Unix系统的BSD分支开发出来的，但现在一般可以移植到其它类Unix系统上：linux和System V的变种都支持套接字。

参考资料：

- [Java线程与线程、进程与进程之间通信方式 | 理想村 | 屠杭镝的博客](https://www.tuhd.top/2017/08/04/2017-08-04-threadandprocess/)
- [JAVA多线程之线程间的通信方式 - hapjin - 博客园](https://www.cnblogs.com/hapjin/p/5492619.html)