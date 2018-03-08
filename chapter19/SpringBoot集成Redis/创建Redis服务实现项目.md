# 创建 Redis 服务实现项目

---

## 项目名称

gaming-server-service-redis

## POM

增加 redis 依赖

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

完整 POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ooqiu.gaming</groupId>
    <artifactId>gaming-server-service-redis</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>gaming-server-service-redis</name>
    <description></description>

    <parent>
        <groupId>com.ooqiu.gaming</groupId>
        <artifactId>gaming-server-dependencies</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <dependencies>
        <!-- Spring Boot Begin -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- Spring Boot End -->

        <!-- Alibaba Begin -->
        <dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
        </dependency>
        <!-- Alibaba End -->

        <!-- Zookeeper Begin -->
        <dependency>
            <groupId>com.101tec</groupId>
            <artifactId>zkclient</artifactId>
        </dependency>
        <!-- Zookeeper End -->

        <!-- Project Begin -->
        <dependency>
            <groupId>com.ooqiu.gaming</groupId>
            <artifactId>gaming-server-service-redis-api</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>com.ooqiu.gaming</groupId>
            <artifactId>gaming-server-commons</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>
        <!-- Project End -->
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.ooqiu.gaming.service.redis.GamingServerServiceRedisApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

## 实现接口

```
package com.ooqiu.gaming.service.redis.api.impl;

import com.alibaba.dubbo.config.annotation.Service;
import com.ooqiu.gaming.service.redis.api.RedisService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;

import java.util.concurrent.TimeUnit;

@Service(version = "1.0.0")
public class RedisServiceImpl implements RedisService {

    @Autowired
    private RedisTemplate<Object, Object> redisTemplate;

    @Override
    public Object get(String key) {
        return redisTemplate.opsForValue().get(key);
    }

    @Override
    public void set(String key, Object value) {
        redisTemplate.opsForValue().set(key, value);
    }

    @Override
    public void set(String key, Object value, long seconds) {
        redisTemplate.opsForValue().set(key, value, seconds, TimeUnit.SECONDS);
    }
}
```

## Application

```
package com.ooqiu.gaming.service.redis;

import com.alibaba.dubbo.container.Main;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class GamingServerServiceRedisApplication {
    public static void main(String[] args) {
        SpringApplication.run(GamingServerServiceRedisApplication.class, args);
        Main.main(args);
    }
}
```

## application.yml

```
spring:
  application:
    name: gaming-server-service-redis
  redis:
    database: 0
    # 单机使用，对应服务器ip
    host: 192.168.75.134
    # 密码，如果没有设置可不配
    # password:
    # 单机使用，对应端口号
    port: 6379
    # 池配置
    pool:
      max-idle: 8
      min-idle: 0
      max-active: 8
      max-wait: -1
   # HA 环境，上生产时使用
   # sentinel:
     # master: mymaster
     # 多个节点用“,”分割
     # nodes: 192.168.75.134:26379

dubbo:
  scan:
    base-packages: com.ooqiu.gaming.service.redis.api
  application:
    id: gaming-server-service-redis
    name: gaming-server-service-redis
  protocol:
    id: dubbo
    name: dubbo
  registry:
    id: zookeeper
    address: zookeeper://192.168.75.132:2181?backup=192.168.75.132:2182,192.168.75.132:2183
```

## application.properties

```
dubbo.protocol.port=20883
```