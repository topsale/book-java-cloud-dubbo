# Redis 客户端与常用命令

---

## 桌面客户端

### FastoRedis

![](/assets/Lusifer1520526817.png)

### Redis Desktop Manager

![](/assets/Lusifer1520527810.png)

## 常用命令

命令参考：http://doc.redisfans.com/

### 连接命令

* AUTH password：验证密码是否正确
* ECHO message：打印字符串
* PING：查看服务是否运行
* QUIT：关闭当前连接
* SELECT index：切换到指定的数据库

### 服务器命令

* CLIENT LIST：获取连接到服务器的客户端连接列表
* CLIENT KILL [ip:port] [ID client-id]：关闭客户端连接
* TIME：返回当前服务器时间
* CONFIG SET parameter value：修改 redis 配置参数，无需重启
* DBSIZE：返回当前数据库的 key 的数量
* DEBUG SEGFAULT：让 Redis 服务崩溃
* FLUSHALL：删除所有数据库的所有 key
* FLUSHDB：删除当前数据库的所有 key
* INFO [section]：获取 Redis 服务器的各种信息和统计数值
* LASTSAVE：返回最近一次 Redis 成功将数据保存到磁盘上的时间，以 UNIX 时间戳格式表示
* MONITOR：实时打印出 Redis 服务器接收到的命令，调试用

### keys 命令

* EXISTS key：检查给定的 key 是否存在
* TYPE key：返回 key 所存储的值的类型

### 字符串命令

* SET key value
* GET key
* MGET KEY1 KEY2：获取一个或者多个给定 key 的值
* MSET key value [key value ...]：同时设置一个或多个 key-value 对
* APPEND key value：* 如果 key 已经存在并且是一个字符串， APPEND 命令将 指定value 追加到改 key 原来的值（value）的末尾