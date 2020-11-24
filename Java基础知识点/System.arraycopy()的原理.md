System中提供了一个native静态方法arraycopy(),可以使用这个方法来实现数组之间的复制。对于一维数组来说，这种复制属性值传递，修改副本不会影响原来的值。对于二维或者一维数组中存放的是对象时，复制结果是一维的引用变量传递给副本的一维数组，修改副本时，会影响原来的数组。

System.arraycopy的函数原型是：
```
public static void arraycopy(Object src,
                             int srcPos,
                             Object dest,
                             int destPos,
                             int length)
```

src表示源数组，srcPos表示源数组要复制的起始位置，desc表示目标数组，length表示要复制的长度。