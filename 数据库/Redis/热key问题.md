所谓热key问题就是，突然有几十万的请求去访问redis上的某个特定key，那么这样会造成流量过于集中，达到物理网卡上限，从而导致这台redis的服务器宕机引发雪崩。
![[热key问题.bmp]]
针对热key的解决方案：
- 提前把热key打散到不同的服务器，降低压力加入二级缓存，提前加载热key数据到内存中，如果redis宕机，走内存查询。