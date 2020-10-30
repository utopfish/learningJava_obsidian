## [[Redis-哈希对象（hash）]]
hash的底层存储有两种数据结构，一种是ziplist，另外一种是hashtable,hash对象只有同时满足以下条件，才会采用ziplist编码：

- hash对象保存的键和值字符串长度都小于64字节
- hash对象保存的键值对数量小于512
#### ziplist存储的结构
![[lt1eqnum.bmp]]
上图中可以看到，当数据量比较小的时候，我们会将所有的key及value都当成一个元素，顺序的存入到ziplist中，构成有序。
#### hashtable存储的结构 
![[xbi1pmj3.bmp]]




**字符串的set key value 和 hash 的区别是什么**

>过期时间，hash没有过期时间set不断的加值有一个问题，dict中有一个属性是dictht ht[2]，主要是> 扩容用的，如果不断的加key，则整体redis内存就需要扩容，扩容就需要基于原有内存增加一倍，内存消耗很大

## Redis-集合对象（set）
set是一个无序的、自动去重的集合数据类型，Set底层用两种数据结构存储，一个是hashtable，一个是inset。
其中hashtable的key为set中元素的值，而value为null
inset为可以理解为数组，使用inset数据结构需要满足下述两个条件：

1. 元素个数不少于默认值512。
2. 元素可以用整型表示
intset的底层结构
```
typedef struct intset {        
	// 编码类型    
	uint32_t encoding;    
	// 集合包含的元素数量    
	uint32_t length;    
	// 保存元素的数组    
	int8_t contents[];
} intset;
```
![[4mzh7ic0.bmp]]
查询方式一般采用二分查找法，实际查询复杂度也就在log(n)
## [[zset底层存储结构]]
zset为有序（有限score排序，score相同则元素字典序），自动去重的集合数据类型，其底层实现为 字典（dict） + 跳表（skiplist），当数据比较少的时候用ziplist编码结构存储。
