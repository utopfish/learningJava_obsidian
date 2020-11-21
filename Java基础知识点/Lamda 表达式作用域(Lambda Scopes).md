访问局部变量

我们可以直接在 lambda 表达式中访问外部的局部变量：
```java
final int num = 1;
Converter<Integer, String> stringConverter =
        (from) -> String.valueOf(from + num);

stringConverter.convert(2);     // 3
```
但是和匿名对象不同的是，这里的变量num可以不用声明为final，该代码同样正确：
```java
int num = 1;
Converter<Integer, String> stringConverter =
        (from) -> String.valueOf(from + num);

stringConverter.convert(2);     // 3
```
不过这里的 num 必须不可被后面的代码修改（即隐性的具有final的语义），例如下面的就无法编译：
```java
int num = 1;
Converter<Integer, String> stringConverter =
        (from) -> String.valueOf(from + num);
num = 3;//在lambda表达式中试图修改num同样是不允许的。
```
访问字段和静态变量

与局部变量相比，我们对lambda表达式中的实例字段和静态变量都有读写访问权限。 该行为和匿名对象是一致的。
```java
class Lambda4 {
    static int outerStaticNum;
    int outerNum;

    void testScopes() {
        Converter<Integer, String> stringConverter1 = (from) -> {
            outerNum = 23;
            return String.valueOf(from);
        };

        Converter<Integer, String> stringConverter2 = (from) -> {
            outerStaticNum = 72;
            return String.valueOf(from);
        };
    }
}
```