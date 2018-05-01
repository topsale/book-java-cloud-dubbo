# RabbitMQ 创建生产者

---

##  创建生产者项目

spring-boot-amqp-provider

## application.yml

```
spring:
  application:
    name: spring-boot-amqp
  rabbitmq:
    host: 192.168.75.133
    port: 5672
    username: rabbit
    password: 123456
```

## 创建队列配置

```
package com.lusifer.spring.boot.amqp.config;

import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 队列配置
 * <p>Title: RabbitMQConfiguration</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/5/1 22:39
 */
@Configuration
public class RabbitMQConfiguration {
    @Bean
    public Queue queue() {
        return new Queue("helloRabbit");
    }
}
```

## 创建消息提供者

```
package com.lusifer.spring.boot.amqp.provider;

import org.springframework.amqp.core.AmqpTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.Date;

/**
 * 提供者
 * <p>Title: HelloRabbitProvider</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/5/1 22:39
 */
@Component
public class HelloRabbitProvider {
    @Autowired
    private AmqpTemplate amqpTemplate;

    public void send() {
        String context = "hello" + new Date();
        System.out.println("Provider: " + context);
        amqpTemplate.convertAndSend("helloRabbit", context);
    }
}
```

## 创建测试用例

```
package com.lusifer.spring.boot.amqp.test;

import com.lusifer.spring.boot.amqp.Application;
import com.lusifer.spring.boot.amqp.provider.HelloRabbitProvider;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = Application.class)
public class AmqpTest {
    @Autowired
    private HelloRabbitProvider helloRabbitProvider;

    @Test
    public void testSender() {
        for (int i = 0; i < 10; i++) {
            helloRabbitProvider.send();
        }
    }
}
```