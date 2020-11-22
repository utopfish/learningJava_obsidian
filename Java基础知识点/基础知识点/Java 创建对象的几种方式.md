## 1. 使用new关键字
太基础以至于完全不想介绍。
```java
User user = new User();
```
## 2. 反射
使用Class类的newInstance方法
```java
User user = User.class.newInstance();
```
使用Constructor类的newInstance方法
```java
Constructor<User> constructor = User.class.getConstructor();
User user = constructor.newInstance();
```
## 3. clone 方法
要使用clone方法，我们需要先实现Cloneable接口并实现其定义的clone方法。

## 4. 反序列化