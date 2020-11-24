HashMap扩容的源码
```
/**
 * Transfers all entries from current table to newTable. 
 */
void transfer(Entry[] newTable, boolean rehash) {
    int newCapacity = newTable.length;
    for (Entry<K,V> e : table) {

        while(null != e) {
            //（关键代码）
            Entry<K,V> next = e.next;
            if (rehash) {
                e.hash = null == e.key ? 0 : hash(e.key);
            }
            int i = indexFor(e.hash, newCapacity);
            e.next = newTable[i];
            newTable[i] = e;
            e = next;
        } // while  

    }
}
```
## 总结
线程B先执行，执行完Entry<K,V> next = e.next;这行代码后挂起，然后线程A完整的执行完整个扩容流程，接着线程B唤醒，继续之前的往下执行，当while循环执行3次后会形成环形链表。
## 细节
假设一个HashMap已经到了Resize的临界点。此时有两个线程A和B，在同一时刻对HashMap进行Put操作： 
![[多线程下头插法可能导致成环.bmp]]
此时达到Resize条件，两个线程各自进行Rezie的第一步，也就是扩容：

假如此时线程B遍历到Entry3对象，刚执行完Entry<K,V> next = e.next;，线程就被挂起。 
对于线程B来说 ： e = Entry3 next =Entry2 
这时候线程A畅通无阻地进行着Rehash，当ReHash完成后，结果如下（图中的e和next，代表线程B的两个引用）：
![[ReHash完成后1.bmp]]

直到这一步，都没问题。接下来线程B恢复，继续执行属于它自己的ReHash。 线程B刚才的状态是：e = Entry3 next = Entry2 
![[hashMap的rehash源码1.bmp]]
当执行到上面这一行时，显然 i = 3，因为刚才线程A对于Entry3的hash结果也是3。 ![[hashMap的rehash源码2.bmp]]

我们继续执行到这两行，Entry3放入了线程B的数组下标为3的位置，并且e指向了Entry2。 
此时e和next的指向如下： 
e =Entry2 next = Entry2 
整体情况如下图所示： 
![[ReHash完成后2.bmp]]
ReHash完成后
![[hashMap的rehash源码3.bmp]]
e = Entry2 
next = Entry3 
整体情况如图所示：
![[ReHash完成后3.bmp]]

接下来执行下面的三行，用头插法把Entry2插入到了线程B的数组的头结点：
![[hashMap的rehash源码4.bmp]]
整体情况如图所示： 
![[ReHash完成后4.bmp]]
第三次循环开始，又执行到红框的代码：

![[hashMap的rehash源码3.bmp]]
e = Entry3 
next = Entry3.next = null

最后一步，当我们执行下面这一行的时候，见证奇迹的时刻来临了 
![[hashMap的rehash源码5.bmp]]

newTable[i] = Entry2 
e = Entry3 
Entry2.next = Entry3 
Entry3.next = Entry2 
链表出现了环形！ 
整体情况如图所示：
![[链表成环.bmp]]

此时，问题还没有直接产生。当调用Get查找一个不存在的Key， 
而这个Key的Hash结果恰好等于3的时候，由于位置3带有环形链表，所以程序将会进入死循环！