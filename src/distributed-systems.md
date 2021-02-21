# 分布式系统

## CAP 定理

参考[CAP 定理的含义](http://www.ruanyifeng.com/blog/2018/07/cap.html)

## 容错、高可用和灾备

参考[容错，高可用和灾备](http://www.ruanyifeng.com/blog/2019/11/fault-tolerance.html)

## 分布式锁

分布式微服务架构，拆分后各个微服务之间为了避免冲突和数据故障而加入的一种锁。

可以基于 MySQL、ZooKeeper、Redis等实现分布式锁，一般互联网公司，大家都习惯用 Redis 做分布式锁。
