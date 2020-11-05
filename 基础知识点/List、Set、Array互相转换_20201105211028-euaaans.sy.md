## 1. Array、List互转

#### Array转List

```java
String[] s = new String[]{"A", "B", "C", "D","E"};
List<String> list = Arrays.asList(s);
```

这里list里面的元素直接是s里面的元素( list backed by the specified array)，换句话就是说：对s的修改，直接影响list。见[[Arrays.asList()使用指南]]

```java
s[0] ="AA";
System.out.println("list: " + list);//输出为:list: [AA, B, C, D, E]
```

#### List转Array

```java
String[] dest = list.toArray(new String[0]);//new String[0]是指定返回数组的类型
System.out.println("dest: " + Arrays.toString(dest));//输出：dest: [AA, B, C, D, E]
```

这里的dest里面的元素不是list里面的元素，换句话就是说：对list中关于元素的修改，不会影响dest。

```java
list.set(0, "Z");
System.out.println("modified list: " + list);//输出：modified list: [Z, B, C, D, E]
System.out.println("dest: " + Arrays.toString(dest));//输出：dest: [AA, B, C, D, E]
```

## 2. List、Set互转

因为`List`和`Set`都实现了`Collection`接口，且`addAll(Collection<? extends E> c)`;方法，因此可以采用`addAll()`方法将`List`和`Set`互相转换；另外，`List`和`Set`也提供了`Collection<? extends E> c`作为参数的构造函数，因此通常采用构造函数的形式完成互相转化。

```java
//List转Set
Set<String> set = new HashSet<>(list);
System.out.println("set: " + set);
//Set转List
List<String> list_1 = new ArrayList<>(set);
System.out.println("list_1: " + list_1);
```

和`toArray()`一样，被转换的`List(Set)`的修改不会对被转化后的`Set（List）`造成影响。

## 3. Array、Set互转

```java
//array转set
s = new String[]{"A", "B", "C", "D","E"};
set = new HashSet<>(Arrays.asList(s));
System.out.println("set: " + set);
//set转array
dest = set.toArray(new String[0]);
System.out.println("dest: " + Arrays.toString(dest));
```

## `Arrays.asList()` 和 `Collection.toArray()`

述列出的互相转换离不开Arrays.asList()和Collection.toArray()两个重要的方法；
