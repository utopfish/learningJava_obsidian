哈希表是复杂度为 O(1) 的数据结构，在 Java 开发中，我们最常见到就是 HashMap 和 Hashtable，但是在并发场景中使用都不够合理。

**HashMap** ：HashMap 是**线程不安全**的，在并发环境下，可能会形成**环状链表**（扩容时可能造成），导致 get 操作时，cpu 空转，所以，在并发环境中使 用HashMap 是非常危险的。
**Hashtable** ： Hashtable 和 HashMap的实现原理几乎一样，差别无非是：
1. Hashtable不允许key和value为null。
2. Hashtable是线程安全的。

但是 Hashtable 线程安全的策略实现代价却太大了，简单粗暴，get/put 所有相关操作都是 synchronized 的，这相当于给整个哈希表加了一把大锁，多线程访问时候，只要有一个线程访问或操作该对象，那其他线程只能阻塞，相当于将所有的操作串行化，在竞争激烈的并发场景中性能就会非常差。
  
  
Hashtable 性能差主要是由于所有操作需要竞争同一把锁，而如果容器中有多把锁，每一把锁锁一段数据，这样在多线程访问时不同段的数据时，就不会存在锁竞争了，这样便可以有效地提高并发效率。这就是 ConcurrentHashMap 所采用的 "分段锁" 思想。
## jdk7和jdk8中ConcurrentHashMap的实现方法
在JDK1.7及其之前ConcurrentHashMap实现线程安全的方法相对比较简单：

- 其内部将数据分为数个“段（Segment）”，其数量和并发级别有关系，具体是“大于等于并发级别的最小的2的幂次”。
- 每个segment使用单独的ReentrantLock（分段锁）。
- 如果操作涉及不同segment，则可以并发执行，如果是同一个segment则会进行锁的竞争和等待。此设计的效率是高于synchronized的。


不过JDK8之后，ConcurrentHashMap舍弃了ReentrantLock，而重新使用了synchronized。其原因大致有一下几点：

- 加入多个分段锁浪费内存空间。生产环境中， map 在放入时竞争同一个锁的概率非常小，分段锁反而会造成更新等操作的长时间等待。
- 为了提高 GC 的效率

主要就是为了应对 HashMap 在并发环境下不安全而诞生的，ConcurrentHashMap 的设计与实现非常精巧，大量的利用了 volatile，final，CAS 等 lock-free 技术来减少锁竞争对于性能的影响。
我们都知道 Map 一般都是数组+链表结构（JDK1.8 该为数组+红黑树）。
![[数组+链表结构.bmp]]
ConcurrentHashMap 避免了对全局加锁改成了局部加锁操作，这样就极大地提高了并发环境下的操作速度，由于 ConcurrentHashMap 在 JDK1.7 和 1.8 中的实现非常不同，接下来我们谈谈 JDK 在 1.7 和 1.8 中的区别。
## JDK1.7 版本的 CurrentHashMap 的实现原理
Segment(分段锁)-减少锁的粒度

ConcurrentHashMap 中的分段锁称为 Segment，它即类似于 HashMap 的结构，即内部拥有一个 Entry 数组，数组中的每个元素又是一个链表,同时又是一个ReentrantLock（Segment继承了ReentrantLock）。
#### 内部结构
ConcurrentHashMap使用分段锁技术，将数据分成一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问，能够实现真正的并发访问。如下图是ConcurrentHashMap的内部结构图：
![[ConcurrentHashMap的内部结构图.bmp]]

从上面的结构我们可以了解到，ConcurrentHashMap 定位一个元素的过程需要进行两次 Hash 操作。

第一次 Hash 定位到 Segment，第二次 Hash 定位到元素所在的链表的头部。
#### 该结构的优劣势

**坏处**是这一种结构的带来的副作用是 Hash 的过程要比普通的 HashMap 要长。

**好处**是写操作的时候可以只对元素所在的 Segment 进行加锁即可，不会影响到其他的 Segment，这样，在最理想的情况下，ConcurrentHashMap 可以最高同时支持 Segment 数量大小的写操作（刚好这些写操作都非常平均地分布在所有的 Segment 上）。
所以，通过这一种结构，ConcurrentHashMap 的并发能力可以大大的提高。
## JDK1.8版本的 CurrentHashMap 的实现原理
JDK8 中 ConcurrentHashMap 参考了 JDK8 HashMap 的实现，采用了数组+链表+红黑树的实现方式来设计，内部大量采用 CAS 操作，这里我简要介绍下[[CAS]]。

CAS：在判断数组中当前位置为null的时候，使用CAS来把这个新的Node写入数组中对应的位置
synchronized ：当数组中的指定位置不为空时，通过加锁来添加这个节点进入数组(链表<8)或者是红黑树（链表>=8）

