**可变性**

- 都是 final 类，都不允许被继承。
- String 长度是不可变的。
- StringBuffer、StringBuilder 长度是可变的。

**线程安全**

- StringBuffer 是线程安全的。
- StringBuilder 不是线程安全的。

但它们两个中的所有方法都是相同的，StringBuffer 在 StringBuilder 的方法之上添加了 synchronized 修饰，保证线程安全。

**性能**

- **StringBuilder 比 StringBuffer 拥有更好的性能**
- 每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象。
- StringBuffer 每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。
- 如果一个 String 类型的字符串，在编译时就可以确定是一个字符串常量，则编译完成之后，字符串会自动拼接成一个常量。此时 String 的速度比 StringBuffer 和 StringBuilder 的性能好的多

**总结**
相同情况下使用 StringBuilder 相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。
