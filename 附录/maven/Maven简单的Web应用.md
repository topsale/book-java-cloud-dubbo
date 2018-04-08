# Maven 简单的 Web 应用

---

## 创建 webapp 目录

在 `src/main` 目录下创建 `webapp` 目录

## 创建 WEB-INF 目录

在 `src/main/webapp` 目录下创建 `WEB-INF` 目录

## 创建 web.xml 文件

在 `src/main/webapp/WEB-INF` 目录下创建 `web.xml` 文件

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <display-name>Maven Study</display-name>
</web-app>
```

## 创建 Servlet

```
package com.lusifer.maven.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class MavenServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        writer.println("Hello Maven Servlet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

## 引入 J2EE 依赖

```
<dependencies>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>

    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

## 配置 Servlet

```
<servlet>
    <servlet-name>MavenServlet</servlet-name>
    <servlet-class>com.lusifer.maven.servlet.MavenServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>MavenServlet</servlet-name>
    <url-pattern>/maven</url-pattern>
</servlet-mapping>
```

## 配置 Tomcat

Run -> Edit Configurations...

![](/assets/Lusifer1513657910.png) 

运行测试

## 创建 index.jsp

在 `src/main/webapp` 目录下创建 `index.jsp` 文件

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Maven</title>
</head>
<body>
    Hello Maven JSP
</body>
</html>
```