Object的equals方法容易抛空指针异常，应使用常量或确定有值的对象来调用 equals。

举个例子：
```java
// 不能使用一个值为null的引用类型变量来调用非静态方法，否则会抛出异常
String str = null;
if (str.equals("SnailClimb")) {
  ...
} else {
  ..
}
```
运行上面的程序会抛出空指针异常，但是我们把第二行的条件判断语句改为下面这样的话，就不会抛出空指针异常，else 语句块得到执行。：

```java
"SnailClimb".equals(str);// false 
```

不过更推荐使用 java.util.Objects#equals(JDK7 引入的工具类)。

```java
Objects.equals(null,"SnailClimb");// false
```

我们看一下java.util.Objects#equals的源码就知道原因了。
```
public static boolean equals(Object a, Object b) {
    // 可以避免空指针异常。如果a==null的话此时a.equals(b)就不会得到执行，避免出现空指针异常。
    return (a == b) || (a != null && a.equals(b));
}
```