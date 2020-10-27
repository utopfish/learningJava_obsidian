## 简介

Arrays.asList() 在平时开发中还是比较常见的，我们可以使用它将一个数组转换为一个List集合。
```java
String[] myArray = {"Apple", "Banana", "Orange"};
List<String> myList = Arrays.asList(myArray);
//上面两个语句等价于下面一条语句
List<String> myList = Arrays.asList("Apple","Banana", "Orange");
```
JDK 源码对于这个方法的说明：
```java
/**
  *返回由指定数组支持的固定大小的列表。此方法作为基于数组和基于集合的API之间的桥梁，
  * 与 Collection.toArray()结合使用。返回的List是可序列化并实现RandomAccess接口。
  */
public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}
```

## 《[[阿里巴巴 Java 开发手册]]》对其的描述

`Arrays.asList()` 将数组转换为集合后,底层其实还是数组，《阿里巴巴Java 开发手册》对于这个方法有如下描述：
![[7yedev9c.bmp]]

## 使用时的注意事项总结

传递的数组必须是对象数组，而不是基本类型。

`Arrays.asList() `是泛型方法，传入的对象必须是对象数组。
```java
int[] myArray = {1, 2, 3};
List myList = Arrays.asList(myArray);
System.out.println(myList.size());//1
System.out.println(myList.get(0));//数组地址值
System.out.println(myList.get(1));//报错：ArrayIndexOutOfBoundsException
int[] array = (int[]) myList.get(0);
System.out.println(array[0]);//1
```
当传入一个原生数据类型数组时，`Arrays.asList()` 的真正得到的参数就不是数组中的元素，而是数组对象本身！

此时List 的唯一元素就是这个数组，这也就解释了上面的代码。

我们使用包装类型数组就可以解决这个问题。
```java
Integer[] myArray = {1, 2, 3};
```
使用集合的修改方法:`add()`、`remove()`、`clear()`会抛出异常。
```java
List myList = Arrays.asList(1, 2, 3);
myList.add(4);//运行时报错：UnsupportedOperationException
myList.remove(1);//运行时报错：UnsupportedOperationException
myList.clear();//运行时报错：UnsupportedOperationException
```
`Arrays.asList()` 方法返回的并不是 `java.util.ArrayList` ，而是 `java.util.Arrays` 的一个内部类,这个内部类并没有实现集合的修改方法或者说并没有重写这些方法。
```java
List myList = Arrays.asList(1, 2, 3);
System.out.println(myList.getClass());//class java.util.Arrays$ArrayList
```
下图是`java.util.Arrays$ArrayList`的简易源码，我们可以看到这个类重写的方法有哪些。
```java
  private static class ArrayList<E> extends AbstractList<E>
        implements RandomAccess, java.io.Serializable
    {
        ...

        @Override
        public E get(int index) {
          ...
        }

        @Override
        public E set(int index, E element) {
          ...
        }

        @Override
        public int indexOf(Object o) {
          ...
        }

        @Override
        public boolean contains(Object o) {
           ...
        }

        @Override
        public void forEach(Consumer<? super E> action) {
          ...
        }

        @Override
        public void replaceAll(UnaryOperator<E> operator) {
          ...
        }

        @Override
        public void sort(Comparator<? super E> c) {
          ...
        }
    }
```
我们再看一下`java.util.AbstractList`的`remove()`方法，这样我们就明白为啥会抛出`UnsupportedOperationException`。
```java
public E remove(int index) {
    throw new UnsupportedOperationException();
}
```
2.1.4. 如何正确的将数组转换为```ArrayList```?

[stackoverflow](https://dwz.cn/vcBkTiTW)

1. 自己动手实现（教育目的）
```java
//JDK1.5+
static <T> List<T> arrayToList(final T[] array) {
  final List<T> l = new ArrayList<T>(array.length);

  for (final T s : array) {
    l.add(s);
  }
  return l;
}
```
```
Integer [] myArray = { 1, 2, 3 };
System.out.println(arrayToList(myArray).getClass());//class java.util.ArrayList
```
2. 最简便的方法(推荐)
```
List list = new ArrayList<>(Arrays.asList("a", "b", "c"))
```
3. 使用 Java8 的`Stream`(推荐)
```
Integer [] myArray = { 1, 2, 3 };
List myList = Arrays.stream(myArray).collect(Collectors.toList());
//基本类型也可以实现转换（依赖boxed的装箱操作）
int [] myArray2 = { 1, 2, 3 };
List myList = Arrays.stream(myArray2).boxed().collect(Collectors.toList());
```
4. 使用 Guava(推荐)

对于不可变集合，你可以使用`ImmutableList`类及其`of()`与`copyOf()`工厂方法：（参数不能为空）
```
List<String> il = ImmutableList.of("string", "elements");  // from varargs
List<String> il = ImmutableList.copyOf(aStringArray);      // from array
```
对于可变集合，你可以使用Lists类及其newArrayList()工厂方法：
```
List<String> l1 = Lists.newArrayList(anotherListOrCollection);    // from collection
List<String> l2 = Lists.newArrayList(aStringArray);               // from array
List<String> l3 = Lists.newArrayList("or", "string", "elements"); // from varargs
```
5. 使用 Apache Commons Collections
```
List<String> list = new ArrayList<String>();
CollectionUtils.addAll(list, str);
```
6. 使用 Java9 的`List.of()`方法(看起来和python最像的简单方法)
```
Integer[] array = {1, 2, 3};
List<Integer> list = List.of(array);
System.out.println(list); /* [1, 2, 3] */
/* 不支持基本数据类型 */
```