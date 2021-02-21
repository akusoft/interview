# Redis

Redis 除了拿来做缓存，还可以用来做分布式锁或消息队列。

## Redis 的常用数据类型

由于 Redis 是键值对数据库，其键的常用类型是 string，所以这里指的是值的常用类型。

- **String**：应用于点赞/反对等的场景
- **Hash**：应用于购物车等场景
- **List**：应用于微信订阅号消息等场景
- **Set**：应用于抽奖/关注社交关系/可能认识的人等场景
- **Sorted Set**（zset）：应用于排行榜等场景

除此之外，还有以下类型：

- bitmap
- HyperLogLog
- geo
- Stream

## 缓存击穿、缓存穿透、缓存雪崩

## Redis 持久化

## Redis 分布式锁

## Redis 常用命令

通用命令：

```text
help @string
del k1
```

String 命令：

```text
set k1 1
get k1

mset k2 v2 k3 v3
mget k2 k3

incr k1
incrby k1 2
decr k1
decrby k1 2

strlen k2

setnx k4 v4
```

Hash 命令：

```text
hset person id 1
hset person name zhang
hget person id
hget person name

hmset person score 98 birth 20201010
hmget person score birth

hincrby person id

hgetall person
hlen person
hdel person
```

List 命令：

```text
lpush list01 1 2 3 4 4 4 5
rpush list01 6 7 8
lrange list01 0 -1
llen  list01
```

Set 命令：

```text
sadd seta 1 1 1 2 3 4 5
smembers seta
srandmember seta
srandmember seta
scard seta
spop seta
scard seta
srem seta 1
sismember seta 1

# 集合运算
sdiff seta setb  # 差集
sinter seta setb  # 交集
sunion seta setb  # 并集
```

Sorted Set 命令：

```text
zadd zseta 100 mov1 20 mov2
zrange zseta 0 10 withscores
zincrby zseta 1 mov1
```

## Redis 集群

## 为什么 Redis 使用单线程

参考：[为什么 Redis 选择单线程模型](https://draveness.me/whys-the-design-redis-single-thread/)
