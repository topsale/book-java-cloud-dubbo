# SpringBoot 配置 CORS

---

## 使用 Java 配置的方式

```
package com.ooqiu.gaming.server.api.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

/**
 * 跨域配置
 * <p>Title: CorsConfiguration</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/8 22:56
 */
@Configuration
public class CORSConfiguration extends WebMvcConfigurerAdapter {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**").allowedOrigins("*")
                .allowedMethods("GET", "HEAD", "POST", "PUT", "DELETE", "OPTIONS")
                .allowCredentials(false).maxAge(3600);
    }
}
```

## 使用注解的方式

```
@CrossOrigin(origins = "*", maxAge = 3600)
```

## 解决跨域后的效果图

![](/assets/Lusifer1520521282.png)