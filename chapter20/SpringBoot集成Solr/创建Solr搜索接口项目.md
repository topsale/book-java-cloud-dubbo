# 创建 Solr 搜索接口项目

---

## 项目名称

gaming-server-service-search-api

## POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ooqiu.gaming</groupId>
    <artifactId>gaming-server-service-search-api</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>gaming-server-service-search-api</name>
    <description></description>

    <parent>
        <groupId>com.ooqiu.gaming</groupId>
        <artifactId>gaming-server-dependencies</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <dependencies>
        <dependency>
            <groupId>com.ooqiu.gaming</groupId>
            <artifactId>gaming-server-domain</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>
    </dependencies>

</project>
```