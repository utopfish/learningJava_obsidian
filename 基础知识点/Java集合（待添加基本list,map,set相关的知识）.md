Java集合框架 (Java Collections Framework, JCF) 也称容器，这里可以类比 C++ 中的 STL，在市面上似乎还没能找到一本详细介绍的书籍。在这里主要对如下部分进行源码分析，及在面试中常见的问题。
Java集合框架提供了数据持有对象的方式，提供了对数据集合的操作。Java 集合框架位于 java.util 包下，主要有三个大类：Collection(接口)、Map(接口)、集合工具类。

### 整体框架 
![[804_1.png]]
![[v5485qzx.bmp]]

[[Arrays.asList()使用指南]]

### List

-` Arraylist`： `Object[]`数组。
- `Vector`：`Object[]`数组。
- `LinkedList`： 双向链表(JDK1.6 之前为循环链表，JDK1.7 取消了循环)。

### Set

 - ` HashSet`（无序，唯一）: 基于 `HashMap` 实现的，底层采用 `HashMap` 来保存元素。
 - `LinkedHashSet`：是 `HashSet` 的子类，并且其内部是通过 `LinkedHashMap` 来实现的。
 - `TreeSet`（有序，唯一）： 红黑树(自平衡的排序二叉树)。

### Map

   - `HashMap`：JDK1.8 以后在解决哈希冲突时，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。
   - `LinkedHashMap`： 继承自 `HashMap`，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。
   -` Hashtable`： 数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的。（同步类，已废弃）
   - `TreeMap`： 红黑树（自平衡的排序二叉树）。
   - `ConcurrentHashMap`：线程安全。

### 线程安全的集合
`java.util.concurrent` 包中提供了很多并发容器：

1. `ConcurrentHashMap`: 可以看作是线程安全的 `HashMap`。
2. `CopyOnWriteArrayList`:可以看作是线程安全的 `ArrayList`，在读多写少的场合性能非常好，远远好于 `Vector`。
3. `ConcurrentLinkedQueue`:高效的并发队列，使用链表实现。可以看做一个线程安全的 `LinkedList`，这是一个非阻塞队列。
4. `BlockingQueue`: 这是一个接口，JDK 内部通过链表、数组等方式实现了这个接口。表示阻塞队列，非常适合用于作为数据共享的通道。
5. `ConcurrentSkipListMap` :跳表的实现。这是一个`Map`，使用跳表的数据结构进行快速查找。


[[HashMap 的长度为什么是 2 的幂次方]]