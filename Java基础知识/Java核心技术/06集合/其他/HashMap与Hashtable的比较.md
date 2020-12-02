#### 与 Hashtable 的比较

- Hashtable 使用 synchronized 来进行同步。
- HashMap 可以插入键为 null 的 Entry。
- HashMap 的迭代器是 [[fail-fast]] 迭代器。
- HashMap 不能保证随着时间的推移 Map 中的元素次序是不变的。
[[HashMap 的长度为什么是 2 的幂次方]]