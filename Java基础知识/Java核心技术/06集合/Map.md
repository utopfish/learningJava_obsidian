- `TreeMap`：线程不同步，基于 **红黑树** （Red-Black tree）的 NavigableMap 实现，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器，当用 Iterator 遍历 TreeMap 时，得到的记录是排过序的。**TreeMap 底层通过红黑树（Red-Black tree）实现**，也就意味着 `containsKey()`, `get()`, `put()`, `remove()` 都有着 `log(n)` 的时间复杂度。其具体算法实现参照了《算法导论》。
- `Hashtable`：**线程安全**，HashMap 的迭代器 \(Iterator\) 是 `fail-fast` 迭代器。**Hashtable 不能存储 NULL 的 key 和 value。**
- `HashMap`：线程不同步。根据 `key` 的 `hashcode` 进行存储，内部使用静态内部类 `Node` 的数组进行存储，默认初始大小为 16，每次扩大一倍。当发生 Hash 冲突时，采用拉链法（链表）。JDK 1.8中：**当单个桶中元素个数大于等于8时，链表实现改为红黑树实现；当元素个数小于6时，变回链表实现。由此来防止hashCode攻击。**Java HashMap 采用的是冲突链表方式。  HashMap 是 Hashtable 的轻量级实现，**可以接受为 null 的键值 (key) 和值 (value)**，而 Hashtable 不允许。
- `LinkedHashMap`：**保存了记录的插入顺序**，在用 Iterator 遍历 LinkedHashMap 时，先得到的记录肯定是先插入的。也可以在构造时用带参数，按照应用次数排序。在遍历的时候会比 HashMap 慢，不过有种情况例外，当 HashMap 容量很大，实际数据较少时，遍历起来可能会比 LinkedHashMap 慢，因为 LinkedHashMap 的遍历速度只和实际数据有关，和容量无关，而 HashMap 的遍历速度和他的容量有关。
- `WeakHashMap`：从名字可以看出它是某种 Map。它的特殊之处在于 **WeakHashMap 里的 entry 可能会被 GC 自动删除**，即使程序员没有调用 `remove()` 或者 `clear()` 方法。 WeakHashMap 的存储结构类似于HashMap
  - 既然有 WeekHashMap，是否有 WeekHashSet 呢？答案是没有！不过 Java Collections 工具类给出了解决方案，`Collections.newSetFromMap(Map<E,Boolean> map)` 方法可以将任何 Map包装成一个Set。



**在多线程使用场景中，应该尽量避免使用线程不安全的 HashMap，而使用线程安全的 ConcurrentHashMap。**