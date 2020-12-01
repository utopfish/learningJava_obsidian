## hashCode()
```java
public native int hashCode()
```
返回哈希值
HashSet 和 HashMap 等集合类使用了 hashCode() 方法来计算对象应该存储的位置，因此要将对象添加到这些集合类中，需要让对应的类实现 hashCode() 方法。


## equals()
```java
public boolean equals(Object obj)
```
两个对象具有等价关系，需要满足以下五个条件：
1. 自反性
2. 对称性
3. 传递性
4. 一致性（多次调用 equals() 方法结果不变）

对任何不是 null 的对象 x 调用 x.equals(null) 结果都为 false

#### 等价与相等
- 对于基本类型，== 判断两个值是否相等，基本类型没有 equals() 方法。
- 对于引用类型，== 判断两个变量是否引用同一个对象，而 equals() 判断引用的对象是否等价。

## toString()

```java
public String toString()
```
默认返回 ToStringExample@4554617c 这种形式，其中 @ 后面的数值为散列码的无符号十六进制表示。
## clone()
```java
protected native Object clone() throws CloneNotSupportedException
```
cloneable，浅拷贝，深拷贝，使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换。Effective Java 书上讲到，最好不要去使用 clone()，可以使用拷贝构造函数或者拷贝工厂来拷贝一个对象。
```java
public class CloneConstructorExample {

    private int[] arr;

    public CloneConstructorExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }

    public CloneConstructorExample(CloneConstructorExample original) {
        arr = new int[original.arr.length];
        for (int i = 0; i < original.arr.length; i++) {
            arr[i] = original.arr[i];
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }
}
```
## getClass()
```
public final native Class<?> getClass()
```

## finalize()
```java
protected void finalize() throws Throwable {}
```
## notify()
```java
public final native void notify()
```
## notifyAll()
```java
public final native void notifyAll()
```
## wait()
```java
public final native void wait(long timeout) throws InterruptedException
```

```java
public final void wait(long timeout, int nanos) throws InterruptedException
```

```java
public final void wait() throws InterruptedException
```