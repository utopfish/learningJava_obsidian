## 如何使用JPA在数据库中非持久化一个字段？

假如我们有有下面一个类：
```
Entity(name="USER")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "ID")
    private Long id;

    @Column(name="USER_NAME")
    private String userName;

    @Column(name="PASSWORD")
    private String password;

    private String secrect;

}
```
如果我们想让secrect 这个字段不被持久化，也就是不被数据库存储怎么办？我们可以采用下面几种方法：

static String transient1; // not persistent because of static
final String transient2 = “Satish”; // not persistent because of final
transient String transient3; // not persistent because of transient
@Transient
String transient4; // not persistent because of @Transient

一般使用后面两种方式比较多，我个人使用注解的方式比较多。