## 介绍

在 Java 动态代理机制中 `InvocationHandler `接口和 `Proxy` 类是核心。

Proxy 类中使用频率最高的方法是：`newProxyInstance()` ，这个方法主要用来生成一个代理对象。
```
    public static Object newProxyInstance(ClassLoader loader,
                                          Class<?>[] interfaces,
                                          InvocationHandler h)
        throws IllegalArgumentException
    {
        ......
    }
```
这个方法一共有 3 个参数：
1. loader :类加载器，用于加载代理对象;
2. interfaces : 被代理类实现的一些接口；
3. h : 实现了 InvocationHandler 接口的对象；

要实现动态代理的话，还必须需要实现`InvocationHandler `来自定义处理逻辑。 当我们的动态代理对象调用一个方法时候，这个方法的调用就会被转发到实现`InvocationHandler` 接口类的` invoke `方法来调用。
```
public interface InvocationHandler {

    /**
     * 当你使用代理对象调用方法的时候实际会调用到这个方法
     */
    public Object invoke(Object proxy, Method method, Object[] args)
        throws Throwable;
}
```
`invoke()` 方法有下面三个参数：

1. proxy :动态生成的代理类;
2. method : 与代理类对象调用的方法相对应;
3. args : 当前 method 方法的参数

也就是说：你通过`Proxy` 类的` newProxyInstance()` 创建的代理对象在调用方法的时候，实际会调用到实现InvocationHandler 接口的类的 `invoke()`方法。 你可以在` invoke() `方法中自定义处理逻辑，比如在方法执行前后做什么事情。
## JDK 动态代理类使用步骤

1. 定义一个接口及其实现类；
2. 自定义 InvocationHandler 并重写invoke方法，在`invoke` 方法中我们会调用原生方法（被代理类的方法）并自定义一些处理逻辑；
3. 通过` Proxy.newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h) `方法创建代理对象；

## 代码示例

这样说可能会有点空洞和难以理解，我上个例子，大家感受一下吧！

1.定义发送短信的接口

```
public interface SmsService {
    String send(String message);
}
```

2.实现发送短信的接口
```
public class SmsServiceImpl implements SmsService {
    public String send(String message) {
        System.out.println("send message:" + message);
        return message;
    }
}
```
3.定义一个 JDK 动态代理类

```import java.lang.reflect.InvocationHandler;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
 * @author shuang.kou
 * @createTime 2020年05月11日 11:23:00
 */
public class DebugInvocationHandler implements InvocationHandler {
    /**
     * 代理类中的真实对象
     */
    private final Object target;

    public DebugInvocationHandler(Object target) {
        this.target = target;
    }


    public Object invoke(Object proxy, Method method, Object[] args) throws InvocationTargetException, IllegalAccessException {
        //调用方法之前，我们可以添加自己的操作
        System.out.println("before method " + method.getName());
        Object result = method.invoke(target, args);
        //调用方法之后，我们同样可以添加自己的操作
        System.out.println("after method " + method.getName());
        return result;
    }
}
```
`invoke() `方法: 当我们的动态代理对象调用原生方法的时候，最终实际上调用到的是` invoke() `方法，然后` invoke() `方法代替我们去调用了被代理对象的原生方法。

4.获取代理对象的工厂类
```
public class JdkProxyFactory {
    public static Object getProxy(Object target) {
        return Proxy.newProxyInstance(
                target.getClass().getClassLoader(), // 目标类的类加载
                target.getClass().getInterfaces(),  // 代理需要实现的接口，可指定多个
                new DebugInvocationHandler(target)   // 代理对象对应的自定义 InvocationHandler
        );
    }
}
```
`getProxy() `：主要通过`Proxy.newProxyInstance（）`方法获取某个类的代理对象

5.实际使用

```
SmsService smsService = (SmsService) JdkProxyFactory.getProxy(new SmsServiceImpl());
smsService.send("java");
```

运行上述代码之后，控制台打印出：

```
before method send
send message:java
after method send
```