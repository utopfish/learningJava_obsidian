BGP（Border Gateway Protocol，边界网关协议）
AS 之间的路由选择很困难，主要是由于：
- 互联网规模很大；
- 各个 AS 内部使用不同的路由选择协议，无法准确定义路径的度量；
- AS 之间的路由选择必须考虑有关的策略，比如有些 AS 不愿意让其它 AS 经过。
BGP 只能寻找一条比较好的路由，而不是最佳路由。
每个 AS 都必须配置 BGP 发言人，通过在两个相邻 BGP 发言人之间建立 TCP 连接来交换路由信息。
![[BGP发言人和自治系统AS的关系.png]]