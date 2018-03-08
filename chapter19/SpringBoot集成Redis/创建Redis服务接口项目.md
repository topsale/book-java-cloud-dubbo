# 创建 Redis 服务接口项目

---

## 项目名称

gaming-server-service-redis-api

## POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ooqiu.gaming</groupId>
    <artifactId>gaming-server-service-redis-api</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>gaming-server-service-redis-api</name>
    <description></description>

</project>
```

## 定义接口

```
package com.ooqiu.gaming.service.redis.api;

/**
 * Redis 服务接口
 * <p>Title: RedisService</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/9 1:26
 */
public interface RedisService {
    public Object get(String key);

    public void set(String key, Object value);

    /**
     * 带过期时间
     * @param key
     * @param value
     * @param seconds 过期时间/秒
     */
    public void set(String key, Object value, long seconds);
}
```