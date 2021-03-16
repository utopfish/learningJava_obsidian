-   spring,spring-boot,Java程序员一般来说都离不开Spring这个框架，常见的一些问题有：
    
#### IoC 和 AOP 是什么？
IoC（Inverse of Control:控制反转）是一种设计思想，就是 **将原本在程序中手动创建对象的控制权，交由Spring框架来管理。** **IoC 容器实际上就是个Map（key，value）,Map 中存放的是各种对象。将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。 IoC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。**
Spring 时代我们一般通过 XML 文件来配置 Bean，后来开发人员觉得 XML 文件来配置不太好，于是 SpringBoot 注解配置就慢慢开始流行起来。

![Spring IoC的初始化过程](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-7/SpringIOC%E5%88%9D%E5%A7%8B%E5%8C%96%E8%BF%87%E7%A8%8B.png)

AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，**却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来**，便于**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。

**Spring AOP就是基于动态代理的**，如果要代理的对象，实现了某个接口，那么Spring AOP会使用**JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候Spring AOP会使用**Cglib** ，这时候Spring AOP会使用 **Cglib** 生成一个被代理对象的子类来作为代理
#### Spring 中的 bean 的作用域有哪些?
- singleton : 唯一 bean 实例，Spring 中的 bean 默认都是单例的。
- prototype : 每次请求都会创建一个新的 bean 实例。
-   request : 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
-   session : 每一次HTTP请求都会产生一个新的 bean，该bean仅在当前 HTTP session 内有效。
-   global-session： 全局session作用域，仅仅在基于portlet的web应用中才有意义，Spring5已经没有了。Portlet是能够生成语义代码(例如：HTML)片段的小型Java Web插件。它们基于portlet容器，可以像servlet一样处理HTTP请求。但是，与 servlet 不同，每个 portlet 都有不同的会话
#### Spring 中的单例 bean 的线程安全问题了解吗？
当多个线程操作同一个对象的时候，对这个对象的成员变量的写操作会存在线程安全问题。常用的 `Controller`、`Service`、`Dao` 这些 Bean 是无状态的。无状态的 Bean 不能保存数据，因此是线程安全的。

### Spring 中的 bean 生命周期?
-   Bean 容器找到配置文件中 Spring Bean 的定义。
-   Bean 容器利用 Java Reflection API 创建一个Bean的实例。
-   如果涉及到一些属性值 利用 `set()`方法设置一些属性值。
-   如果 Bean 实现了 `BeanNameAware` 接口，调用 `setBeanName()`方法，传入Bean的名字。
-   如果 Bean 实现了 `BeanClassLoaderAware` 接口，调用 `setBeanClassLoader()`方法，传入 `ClassLoader`对象的实例。
-   与上面的类似，如果实现了其他 `*.Aware`接口，就调用相应的方法。
-   如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessBeforeInitialization()` 方法
-   如果Bean实现了`InitializingBean`接口，执行`afterPropertiesSet()`方法。
-   如果 Bean 在配置文件中的定义包含 init-method 属性，执行指定的方法。
-   如果有和加载这个 Bean的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessAfterInitialization()` 方法
-   当要销毁 Bean 的时候，如果 Bean 实现了 `DisposableBean` 接口，执行 `destroy()` 方法。
-   当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含 destroy-method 属性，执行指定的方法。
常见的有 2 种解决办法：
1.  在类中定义一个 `ThreadLocal` 成员变量，将需要的可变成员变量保存在 `ThreadLocal` 中（推荐的一种方式）。
2.  改变 Bean 的作用域为 “prototype”：每次请求都会创建一个新的 bean 实例，自然不会存在线程安全问题。

#### MVC 工作原理
![SpringMVC运行原理](http://my-blog-to-use.oss-cn-beijing.aliyuncs.com/18-10-11/49790288.jpg)
    
 ####   Spring 事务
 编程式事务，在代码中硬编码。(不推荐使用)
声明式事务，在配置文件中配置（推荐使用）
声明式事务又分为两种：基于 XML 的声明式事务，基于注解的声明式事务。
   -   Spring 中的 bean 生命周期?
    -   Spring 事务中的隔离级别有哪几种?
    -   Spring 事务中哪几种事务传播行为?
    -   SpringBoot相比于Spring区别有哪些？
    -   Spring Boot 的自动配置是如何实现的?
    -   Spring的动态代理有哪些？


#### mybatis,国内常见的开发套路就是SSM,mybatis就是M,如果你在简历上写上那么也是会必不可少的：
 -   `#{}`和`${}`的区别是什么？
 `#{}`：参数占位符，防止sql注入；`${}`sql拼接符号，是String的替换。能用`#{}`就用；表名做变量时必须用`${}`，因为占位符替换字符串时会带上单引号，导致sql语法出错。
   
  -   Xml 映射文件中，常见的标签有哪些？
`insert` – 映射插入语句
 `update` – 映射更新语句
`delete` – 映射删除语句
`select` – 映射查询语句
`cache` – 对给定命名空间的缓存配置。
`sql` – 可被其他语句引用的可重用语句块，这个元素可以被用来定义可重用的 SQL 代码段。
`resultMap` – 是最复杂也是最强大的元素，用来描述如何从数据库结果集中来加载对象。ResultMap 的设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句只需要描述它们的关系就行了。
    -   Mybatis 是如何进行分页的？
	1.Mybatis 使用 RowBounds 对象进行分页，它是针对 ResultSet 结果集执行的内存分页，而非物理分页。
	2.基于拦截器拦截待执行SQL语句，然后修改最终执行SQL语句。属于物理分页。
    -   Mybatis 的插件运行原理，写一个插件？
    -   xml文件如何和dao接口进行一一对应？(动态代理)