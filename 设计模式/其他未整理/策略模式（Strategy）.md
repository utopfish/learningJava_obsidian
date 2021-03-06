定义一系列算法，封装每个算法，并使它们可以互换。
策略模式可以让算法独立于使用它的客户端。


### 实现
设计一个鸭子，它可以动态地改变叫声。这里的算法族是鸭子的叫声行为。
```java
public interface QuackBehavior {
    void quack();
}
```
```java
public class Quack implements QuackBehavior {
    @Override
    public void quack() {
        System.out.println("quack!");
    }
}
```
```java
public class Squeak implements QuackBehavior{
    @Override
    public void quack() {
        System.out.println("squeak!");
    }
}
```
```java
public class Duck {
    private QuackBehavior quackBehavior;
    public void performQuack() {
        if (quackBehavior != null) {
            quackBehavior.quack();
        }
    }
    public void setQuackBehavior(QuackBehavior quackBehavior) {
        this.quackBehavior = quackBehavior;
    }
}
```
```java
public class Client {
    public static void main(String[] args) {
        Duck duck = new Duck();
        duck.setQuackBehavior(new Squeak());
        duck.performQuack();
        duck.setQuackBehavior(new Quack());
        duck.performQuack();
    }
}
```
```
squeak!
quack!
```

