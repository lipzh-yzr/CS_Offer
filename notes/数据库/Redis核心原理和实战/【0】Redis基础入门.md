# 目录
> Redis 基础入门教程


目录

[TOC]



## Redis 简介

Redis 是完全开源免费的，遵守BSD协议，是一个高性能的key-value数据库。
Redis 与其他 key - value 缓存产品有以下三个特点：

- **Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。**
- **Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。**
- **Redis支持数据的备份，即master-slave模式的数据备份。**

## Redis 优势

- **性能极高 – Redis能读的速度是 110000次/s,写的速度是 81000次/s 。**
- **丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets**
  **数据类型操作。**
- **原子 – Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。**
- **丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。**

### Redis与其他key-value存储有什么不同？

- Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。
- Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，因为数据量不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。

# 如何更好的学习 Redis 

很多技术人都有一个误区，那就是，只关 注零散的技术点，没有建立起一套完整的知识框架，缺乏系统观，但是，系统观其实是至 关重要的。从某种程度上说，在解决问题时，拥有了系统观，就意味着你能有依据、有章 法地定位和解决问题。

Redis 知识全景图都包括什么呢？简单来说，就是“两大维度，三大主 线”。

[![WmNAPS.png](https://z3.ax1x.com/2021/07/15/WmNAPS.png)](https://imgtu.com/i/WmNAPS)

技术点是零碎的，其实你完全可以按照这三大主线，给它们分下类，就像图片中展示 的那样，具体如下：

**高性能主线，**包括线程模型、数据结构、持久化、网络框架；

**高可靠主线，**包括主从复制、哨兵机制； 

**高可扩展主线，**包括数据分片、负载均衡。

## Redis 数据类型

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

### String（字符串）

string 是 redis 最基本的类型，你可以理解成与 Memcached 一模一样的类型，一个 key 对应一个 value。
string 类型是二进制安全的。意思是 redis 的 string 可以包含任何数据。比如jpg图片或者序列化的对象 。
string类型是 Redis 最基本的数据类型，一个键最大能存储 512MB。

#### 实例

```shell
redis 127.0.0.1:6379> SET name "runoob"
OK
redis 127.0.0.1:6379> GET name
"runoob"
```

在以上实例中我们使用了 Redis 的 SET 和 GET 命令。键为 name，对应的值为 runoob。
注意：一个键最大能存储 512 MB。

### Hash（哈希）

Redis hash 是一个键名对集合。
Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。

#### 实例

```shell
127.0.0.1:6379> HMSET user:1 username runoob password runoob points 200
OK
127.0.0.1:6379> HGETALL user:1
1) "username"
2) "runoob"
3) "password"
4) "runoob"
5) "points"
6) "200"
```

以上实例中 hash 数据类型存储了包含用户脚本信息的用户对象。 实例中我们使用了 Redis HMSET, HGETALL 命令，user:1 为键值。
每个 hash 可以存储 2的32 -1 键值对（40多亿）。

### List（列表）

Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。

#### 实例

```shell
redis 127.0.0.1:6379> lpush runoob redis
(integer) 1
redis 127.0.0.1:6379> lpush runoob mongodb
(integer) 2
redis 127.0.0.1:6379> lpush runoob rabitmq
(integer) 3
redis 127.0.0.1:6379> lrange runoob 0 10
1) "rabitmq"
2) "mongodb"
3) "redis"
redis 127.0.0.1:6379>
```

列表最多可存储 232 - 1 元素 (4294967295, 每个列表可存储40多亿)。

### Set（集合）

Redis的Set是string类型的无序集合。
集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

#### sadd 命令

添加一个string元素到,key对应的set集合中，成功返回1,如果元素已经在集合中返回0,key对应的set不存在返回错误。

```shell
sadd key member
```

#### 实例

```shell
redis 127.0.0.1:6379> sadd runoob redis
(integer) 1
redis 127.0.0.1:6379> sadd runoob mongodb
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabitmq
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabitmq
(integer) 0
redis 127.0.0.1:6379> smembers runoob

1) "rabitmq"
2) "mongodb"
3) "redis"
```

注意：以上实例中 rabitmq 添加了两次，但根据集合内元素的唯一性，第二次插入的元素将被忽略。
集合中最大的成员数为 232 - 1(4294967295, 每个集合可存储40多亿个成员)。

### zset(sorted set：有序集合)

Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。
不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。
zset的成员是唯一的,但分数(score)却可以重复。

#### zadd 命令

添加元素到集合，元素在集合中存在则更新对应score

```
zadd key score member
```

#### 实例

```shell
redis 127.0.0.1:6379> zadd runoob 0 redis
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 mongodb
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabitmq
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabitmq
(integer) 0
redis 127.0.0.1:6379> ZRANGEBYSCORE runoob 0 1000

1) "redis"
2) "mongodb"
3) "rabitmq"
```

## Redis 安装

### Windows 安装

**下载地址：**<https://github.com/dmajkic/redis/downloads>。

下载到的 Redis 支持 32bit 和 64bit。根据自己实际情况选择，将 64bit 的内容 cp 到自定义盘符安装目录取名 redis。 如 C:\reids

打开一个 cmd 窗口 使用 cd命令切换目录到 C:\redis 运行 **redis-server.exe redis.conf** 。

如果想方便的话，可以把 redis 的路径加到系统的环境变量里，这样就省得再输路径了，后面的那个redis.conf 可以省略，如果省略，会启用默认的。输入之后，会显示如下界面：

![Redis 安装](https://code.ziqiangxuetang.com/media/uploads/2014/11/redis-win.jpg)

这时候别启一个cmd窗口，原来的不要关闭，不然就无法访问服务端了。

切换到redis目录下运行 **redis-cli.exe -h 127.0.0.1 -p 6379** 。

设置键值对 **set myKey abc**

取出键值对 **get myKey**

![Redis 安装](https://code.ziqiangxuetang.com/media/uploads/2014/11/redis-win2.jpg)

------

## Linux 下安装

**下载地址：**[http://redis.io/download](https://redis.io/download)，下载最新文档版本。

本教程使用的最新文档版本为 2.8.17，下载并安装：

```shell
$ wget http://download.redis.io/releases/redis-2.8.17.tar.gz
$ tar xzf redis-2.8.17.tar.gz
$ cd redis-2.8.17
$ make
```

make 完后 redis-2.8.17 目录下会出现编译后的 redis 服务程序 redis-server ，还有用于测试的客户端程序 redis-cli

下面启动 redis 服务.

```shell
$ ./redis-server
```

注意这种方式启动 redis 使用的是默认配置。也可以通过启动参数告诉 redis 使用指定配置文件使用下面命令启动。

```shell
$ ./redis-server redis.conf
```

redis.conf 是一个默认的配置文件。我们可以根据需要使用自己的配置文件。

启动 redis 服务进程后，就可以使用测试客户端程序 redis-cli 和 redis 服务交互了。 比如：

```shell
$ ./redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

------

## Ubuntu 下安装

在 Ubuntu 系统安装 Redis 可以使用以下命令:

```shell
$sudo apt-get update
$sudo apt-get install redis-server
```

![Ubuntu16安装Redis.PNG](https://i.loli.net/2019/03/04/5c7cd0b0f3a91.png)

### 启动 Redis

```shell
$redis-server
```

![Ubuntu16安装Redis然后启动.PNG](https://i.loli.net/2019/03/04/5c7cd1300527d.png)

### 查看 redis 是否启动？

```shell
$redis-cli
```

![Ubuntu16安装Redis然后启动然后打开命令.PNG](https://i.loli.net/2019/03/04/5c7cd1663a806.png)

以上命令将打开以下终端：

```shell
redis 127.0.0.1:6379>
```

127.0.0.1 是本机 IP ，6379 是 redis 服务端口。现在我们输入 PING 命令。

```shell
redis 127.0.0.1:6379> ping
PONG
```

以上说明我们已经成功安装了redis。

# Redis 配置

Redis 的配置文件位于 Redis 安装目录下，文件名为 redis.conf。

你可以通过 **CONFIG** 命令查看或设置配置项。

------

语法Redis CONFIG 命令格式如下：`redis 127.0.0.1:6379> CONFIG GET CONFIG_SETTING_NAME`

### 实例

```shell
redis 127.0.0.1:6379> CONFIG GET loglevel

1) "loglevel"
2) "notice"
```



更多的 Redis 命令和高级教程，参考  http://www.runoob.com/redis/redis-conf.html

# redis 问题画像图

[![Wmtl1H.png](https://z3.ax1x.com/2021/07/15/Wmtl1H.png)](https://imgtu.com/i/Wmtl1H)
