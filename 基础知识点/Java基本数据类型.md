#### Java中的几种基本数据类型是什么？对应的包装类型是什么？各自占用多少字节呢？
Java中有**8**种基本数据类型，分别为：
1. 6种数字类型 ：byte、short、int、long、float、double
2. 1种字符类型：char
3. 1种布尔型：boolean。 

这八种基本类型都有对应的包装类分别为：Byte、Short、Integer、Long、Float、Double、Character、Boolean

| 基本类型 | 位数 | 字节 | 默认值  |
| :------- | :--- | :--- | ------- |
| int      | 32   | 4    | 0       |
| short    | 16   | 2    | 0       |
| long     | 64   | 8    | 0L      |
| byte     | 8    | 1    | 0       |
| char     | 16   | 2    | 'u0000' |
| float    | 32   | 4    | 0f      |
| double   | 64   | 8    | 0d      |
| boolean  | 1    |      | false   |


对于boolean，官方文档未明确定义，它依赖于 JVM 厂商的具体实现。逻辑上理解是占用 1位，但是实际中会考虑计算机高效存储因素。

[[自动装箱与拆箱]]



[[8种基本类型的包装类和常量池]]
