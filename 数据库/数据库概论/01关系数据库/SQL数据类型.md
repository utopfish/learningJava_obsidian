## 基本类型
- char(n)：固定长度的字符串，用户指定长度 n，也可以用全称 character。
- varchar(n)：可变长度的字符串，用户指定最大长度 n，等价于全称 character varying。
- int：整数类型，等价于全称 integer。
- smallint：小整数类型。
- numeric(p,d)：定点数，精度由用户指定，这个数有 p 位数字（加上一个符号位），其中 d 位数字在小数点右边。numeric(3,1) 可以精确存储 44.5，但不能精确存储 444.5 或 0.32 这样的数。
- real，double precision：浮点数与双精度浮点数，精度与机器有关。
- float(n)：精度至少为 n 位的浮点数。
[[char和varchar的区别]]
## 日期和时间类型
- data：日历日期，包括年（四位）、月和日。
- time：一天中的时间，包括小时、分和秒。可用 time(p) 表示秒后小数点数字位，默认为 0。通过指定 time with timezone 可用一起存储逝去信息。
- timestamp：data 和 time 的组合，可用 timestamp(p) 存储秒后小数点后数字位数。默认为 6。