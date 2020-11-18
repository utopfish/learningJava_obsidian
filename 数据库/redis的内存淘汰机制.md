假设redis每次定期随机查询key的时候没有删掉，这些key也没有做查询的话，就会导致这些key一直保存在redis里面无法被删除，这时候就会走到redis的内存淘汰机制。
1. volatile-lru：从已设置过期时间的key中，移出最近最少使用的key进行淘汰。
2. volatile-ttl：从已设置过期时间的key中，移出将要过期的。
3. keyvolatile-random：从已设置过期时间的key中随机选择key淘汰。
4. allkeys-lru：从key中选择最近最少使用的进行淘汰。
5. allkeys-random：从key中随机选择key进行淘汰。
6. noeviction：当内存达到阈值的时候，新写入操作报错

